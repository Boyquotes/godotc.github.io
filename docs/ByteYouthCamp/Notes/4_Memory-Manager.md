# 内存管理

- 分块
- 缓存

预分配
p --> mspan --> mcentral

##  优化


### Balanced GC
- 每个 g 绑定一大块内存
- GAB 用于 noscan 较小的内存分配
- 压栈方式的分配内存
![](attachments/Pasted%20image%2020230119215422.png)

- 分配大的时候，合并分配多个GAB
	- GAB 延迟释放的问题
 
##  编译
###  静态分析
![](attachments/Pasted%20image%2020230119215948.png)
###  inline
###  逃逸分析
> 分析代码中指针的动态作用域： 指针在何处可以被访问
![](attachments/Pasted%20image%2020230119220922.png)
 
 - 优化
未逃逸申请在栈上


> 分析工具: micro-benchmark
 