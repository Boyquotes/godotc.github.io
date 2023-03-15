# 第一课Golang入门

这是我参与「第五届青训营 」笔记创作活动的第一天

**References**:
> Go语言圣经: https://books.studygolang.com/gopl-zhk/

## Notes
### 1.  struct 与json

在结构体的成员声明之后可以写上tag来标注 `json`的 `key` 以及格式，此后就可以调用json包中的`Marshal` 与`Unmarshal`方法来进行快捷的json格式序列化或者反序列化，为开发提供了便利。
```go
type User struct{
	ID uint32 `json:"id"`
	Name string `json:"name"`
	Details string `json:"details,omitempty"`
}

var A = User{ID:001, Name："小明" }

bin,err := json.Marshal(A)
if err!=nil{
	panic(err)
}

```

### 2. Slice
- 即变长的序列
- 似乎是连续的内存段(还未深入了解), 类似于cpp的vector和stringview, span的结合
```go
var names []string
var ids []int

slc := make([]stirng, 3)
delte names
delte ids
delte slc
```
- 一个slice由`ptr`,  `len` , `capacity` 三部分组成 (首地址， 当前长度，容量大小)
- `slice [i:j]` 表示从 从`i` 到 `j-1` 的子序列
- 追加内容
```go
var alpha = []stirng {"a","b","c"}
alpha = append(alpha, "d")
```
- 不要一直使用slice
	- 如果一个slice引用了`origin slice` 那么 `O-S` 如果一直被 slice 引用，将会得不到释放。因此， 在某些情况下 `make&copy`  > `re-slice`。
	- golang 是一门有GC的语言，算法类似于标记-清除，如果小的slice引用了大的内存段，引用计数就不会减少至0， 即不会被标记上

### 3. 多返回值，
便捷的语言特性，不用定义额外的结构体，在函数声明完参数之后用`()` 包裹多个返回值
```go
func fun(arg1,arg2 int) (int,int,error){
	return  arg1, arg2, nil
}
```

### 4. Wait Group

对一堆的协程进行于类似信号量的操作， 对并发任务做同步, 提供的3个接口
- Add : +1
- Done : -1
- Wait : **阻塞**直到 waitgroup值为0

### 5.  获取命令行参数，环境变量
类似于从C的main函数的参数中获取值
```go
func foo(){
	var args = os.Args // 类型为 []string
	var arg1 = args[1]
	arg2,err := strconv.Atoi(args[2])
}

func bar(){
	var path = os.GetEnv("PATH")	
	var path = os.GetEnv("GOROOT")	
}

```

## Additional
### X. 索引

> refs: https://www.baike.com/wikiid/5527083834876297305?prd=result_list&view_id=20u8ge633gwrnk

>索引是为了加速对表中数据行的检索而创建的一种分散的存储结构。索引是针对表而建立的，它是由数据页面以外的索引页面组成的，每个索引页面中的行都会含有逻辑指针，以便加速检索物理数据。

#### X.1 作用
- 在数据库系统中建立索引主要有以下作用：
	1. 快速取数据；
	2. 保证数据记录的唯一性；
	3. 实现表与表之间的参照完整性；
	4. 在使用ORDER by、croup by子句进行数据检索时，利用索引可以减少排序和分组的时间。

#### X.2  优缺点
- 优点
	1.大大加快数据的检索速度;
	2.创建唯一性索引，保证数据库表中每一行数据的唯一性;
	3.加速表和表之间的连接;
	4.在使用分组和排序子句进行数据检索时，可以显著减少查询中分组和排序的时间。

- 缺点
	1.索引需要占物理空间。
	2.当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，降低了数据的维护速度。
 
####  X.3 索引类型

1. 普通索引
普通的索引通过如下方式创建与修改:
```sql
CREATE INDEX <index_name> On <TABLE> <list of row>
ALTER TABLE <TABLE> ADD INDEX <index_name> <list of row>

"Appoint when creat table"
CREATE TABLE <TABLE> (
 ...
), INDEX  <index_name> <list of row>
```

2. 唯一索引
不允许2行有相同索引值的索引 (即重复key)
>例如，如果在 employee 表中职员的姓 (lname) 上创建了唯一索引，则任何两个员工都不能同姓。
```sql
CREATE UNIQUE INDEX <index_name> On <TABLE> <list of row>
ALTER TABLE <TABLE> ADD UNIQUE <index_name> <list of row>

"Appoint when creat table"
CREATE TABLE <TABLE> (
 ...
), UNIQUE  <index_name> <list of row>
```

3.  主键索引
定义 `primiary key` 时候自动创建, 要求每个都是unique的
在查询时使用主键索引，可以对数据进行快速访问

4. 候选索引
同样要求unique，作为主键（排序等）的候补
可以为每个表建立多个候选索引

5. 聚集索引
`CLUSTERED`
表中行的物理顺序与逻辑（索引）顺序相同，一个表只能包含一个聚集索引
 > 索引不是聚集索引，则表中行的物理顺序与键值的[逻辑顺序](https://www.baike.com/wikiid/1680808987204377087?from=wiki_content&prd=innerlink)不匹配。与[非聚集索引](https://www.baike.com/wikiid/8545432258122618876?from=wiki_content&prd=innerlink)相比，聚集索引通常提供更快的数据访问速度。聚集索引更适用于对很少对基表进行增删改操作的情况。
 
>如果在表中创建了主键约束，SQL Server将自动为其产生唯一性约束。在创建主键约束时，指定了CLUSTERED关键字或干脆没有制定该关键字，SQL Sever将会自动为表生成唯一聚集索引。

6. 非聚集索引
> 也叫非簇索引，在非聚集索引中，数据库表中记录的物理顺序与索引顺序可以不相同。一个表中只能有一个聚集索引，但表中的每一列都可以有自己的非聚集索引。如果在表中创建了主键约束，SQL Server将自动为其产生唯一性约束。在创建主键约束时，如果制定CLUSTERED关键字，则将为表产生唯一聚集索引。

## 不太理解的地方
1. socks5 协议的字段与变长字段
2. 缺少了继承与多态等OO语言特性, 函数也无法重载
3. 查阅相关资料后发现自己对与数据库相关的基础还有很多欠缺

