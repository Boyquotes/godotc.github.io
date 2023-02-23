
# [Introduction](../../../Library/OpenCASCADE/Introduction.md)

模块-库-包-类


- `BRep` : Boundary-Representation 边界表示, BRep 模型是由 拓扑（Topology） 和几何（Geometry）来表示的

- `Topology`
	- Vertex  一个点
	- Edge  两点得到一边
	- Wire:  Close loop(闭合的环) 由相连的Edge形成
	- Face:  由 Wire 限定得到的 **有界** 面
	- Shell: 有多个相连Face 形成的壳
	- Solid: 由shell限定得到的一个闭合体
	- Compound Solid : 多个面相连的Solid 体
	![](attachments/Pasted%20image%2020221115094152.png)
![](attachments/Pasted%20image%2020221115095759.png)
- Module Draw Test Harness


## 选择导航模式


## 画一个瓶子
![](attachments/Pasted%20image%2020221115172605.png)
![](attachments/Pasted%20image%2020221115172555.png)
 - 定义出一部分头机构

![](attachments/Pasted%20image%2020221115172618.png)

![](attachments/Pasted%20image%2020221115180319.png)

- 通过反射现有的导线来计算新的线
- 将反射线条添加到初始线
![](attachments/Pasted%20image%2020221115183211.png)
- 从底座向上拉伸
 ![](attachments/Pasted%20image%2020221115184352.png)
 ![](attachments/Pasted%20image%2020221115184431.png)
- 添加瓶颈
![](attachments/Pasted%20image%2020221115190520.png)
- 掏空内部
![](attachments/Pasted%20image%2020221115193733.png)



## 立方体、圆柱、球、圆锥、圆环
