# 第二课 工程进阶

这是我参与「第五届青训营 」笔记创作活动的第2天

> 上一篇: https://juejin.cn/post/7188971864830509114

## 1. Channel
-  原理是共享内存，unix中的mmap并且加锁?
-  提倡是通过channel来共享内存*而不是*通过共享内存实现通信
```go
c1 := make(chan int)
c2 := make(chan int, 2) // 有缓存
```
![](attachments/Pasted%20image%2020230116132843.png)

- 可以在不同的子协程之间通过channel来同步进度， 此外还会用到 [Wait Group](1_Golang.md#4.%20Wait%20Groupk)
```go
func ch(){
	ch := make(chan int, 5)
	for i:= 0 ; i<5; i++{
		go func(i int){
			Println(i)
			c <- i
		}(i)
	}
	// 避免主线程结束
	for i:=0; i<100; i++{
		<-c
	}
}

func wg(){
	wg := sync.WaitGroup{}
	wg.add(5)
	for i:= 0 ; i<5; i++{
		go func(i int){
			Println(i)
			wg.Done()
		}(i)
	}
	wg.Wait()
}
```

## 2. 项目管理， 依赖管理

### 2.1 管理方式
> Historical Evolution: GOPATH -> Go Vendor -> Go Module

- GOPATH 即环境变量, go 在import时会在GOPATH中去找（与linux包一样，版本控制不太好做)
- Go Vendor 了解的较少, 看起来是类似于 `node_modules` 一样 (不同版本可能不兼容, 或许也有重复包空间占用的问题)
- Go Module 目前一直在用的方式
	 - 有直接依赖与间接依赖之分， 不同版本在`Minor` 之间可以兼容
### 2.2 Proxy
- 中心的一个依赖包仓库管理站点
- 缓存不同代码管理站点的包供开发者拉取(不全是代理，怪不得我有次安装protobuf半天拉不下来)

### 2.3 一些工具
1. `go get`
```sh
$ go get abc.xyz/pkg@<tag>
tags: 
- updaet 默认
- none 删除历k
- vX.y.z tag版本
- 2wexaf hash码，特定的commit
- master 
```
2. `go mod`
```sh
$ go mod init
$ go mod download
$ go mod tidy # 去掉不用的依赖，添加需要的依赖
```

## 3. Test

### 3.1 unit test
- 期望 - assert 输入与输出
- 测试文件的规范
	- 测试函数的命名规范
	```go
	func TestXxx(* testing.T){ //测试函数
			
	}

	func TestHelloWorld(t *test.T){
		output:= SayHello()
		expect := "hello world"
		assert.Equal(t, expect, output)	
	}
	```
	-  TestMain函数， 进行测试环境的搭建工作
	```go
	func TestMain(m *testing.M){
		// 测试之前进行数据装载，配置初始化
		code:= m.Run()
		// 测试后释放资源该Close的都要Close掉
		os.Exit(code)
	}
	```
 - 如何运行单元测试
```sh
$ go test <flags> <packages>
```
- 用test覆盖率来评判
	- 一般为 50%~60%
	- 测试分支需要独立，全面覆盖
	- 粒度要小
 
### 3.2 Mock

> frame:  bouk/monkey
> ref: https://www.liwenzhou.com/posts/Go/unit-test-4/

#### 为函数与方法进行打桩
- 什么是打桩?
	- 在运行时重写exe，将目标函数或方法的实现跳转到桩实现
- 测试时需要 `-gcflags=-1` 关闭内联优化
- 制造测试用的数据与参数
---
####  对函数打桩
```go
// func.go
func Default(uid int64) string{
	u, err := varys.GetInfoByUID(uid)
	if err != nil {
		log.Fatalf(err.Error())
	}

	return fmt.Sprintf("hello %s\n", u.Name)
}
```
 对 Default 函数中varys.GetInfoByUID 进行打桩
  无论传入的uid是多少，都返回 `&varys.UserInfo{Name: "zhangsan"}, nil`
```go
// func_test.go
func TestDefault(t *testing.T) {
	monkey.Patch(varys.GetInfoByUID, func(int64)(*varys.UserInfo, error) {
		return &varys.UserInfo{Name: "zhangsan"}, nil
	})

	ret := Default(abcdefg)
	if !strings.Contains(ret, "zhangsan"){
		t.Fatal()
	}
}
```

#### 对方法Mock
```go
// method_test.go
func TestUser_GetInfo(t *testing.T) {
	var u = &User{
		Name:     "q1mi",
		Birthday: "1990-12-20",
	}

	// 为对象方法打桩
	monkey.PatchInstanceMethod(reflect.TypeOf(u), "CalcAge", func(*User)int {
		return 18
	})

	ret := u.GetInfo()  // 内部调用u.CalcAge方法时会返回18
	if !strings.Contains(ret, "朋友"){
		t.Fatal()
	}
}
```

###  3.3 Benchmark
- 测试普通与并行情况下的性能
-  入参为`b* test.B`
```sh
$ go test -bench=<test_dir>
```
	

##  不太理解的地方
1. benchmark 的定义方式 --> 查阅文档
2. Golang 使用 MVC 的packge包命名与依赖关系
