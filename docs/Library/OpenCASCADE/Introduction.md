
> see this before read: https://zhuanlan.zhihu.com/p/437332699

## TopoDS_Shape
![](https://pic3.zhimg.com/80/v2-583f129f2f0989a3e449abfc9cf26b16_720w.jpg)

- TopoDS_Shape 下有：
	1. location
	2. Orientation
	3. TopoDS_{TShape}

![](https://pic1.zhimg.com/80/v2-0db7da78eefc04ac6f22cd5eea69345c_720w.jpg)

```cpp
1. Location() [2/2]
void TopoDS_Shape::Location	(	const TopLoc_Location & 	theLoc	)	 inline
Sets the shape local coordinate system.(位置）
 
2.Orientation() [2/2]
void TopoDS_Shape::Orientation(	TopAbs_Orientation 	theOrient)inline
Sets the shape orientation.(朝向)
 
3.TShape() [1/2]
const Handle< TopoDS_TShape >& TopoDS_Shape::TShape	(		)	const inline
Returns a handle to the actual shape implementation。(句柄)
```
### TopoDS_TShape

![](https://pic3.zhimg.com/80/v2-dbd72797a5a238b818797004429dcada_720w.webp)

- `TopoDS_TShape` 为一个容器
- `TopoDS_TShape`下 有一系列 `TopoDS...` Prefix 的 shape: vertex edge face wire

#### Brep包含的点、线、面
- 每一个 Topo shape 有 有对应的 **Brep**  shape
- `BRep_<shape>` 下有两个属性
	1. Tolerance
	2. 对应的 shape: (vertex)3D point, (edge)list of representations, (face)surface-trianglation....

#### Tolerance
每个shape的每个顶点位置的球，其他的图形的点在这个球内
![](https://pic2.zhimg.com/80/v2-7109b650cd6b76ccea12eaebd7084509_720w.webp)

Face tolerance <= Edge tolerance <= vertex tolerance
其中 edge 位于 face 上， 而 vertex 位于edge 上

### Orientation
- 表示面法向 和 曲面法线的关系:
	- TopAbs_FORWARD 面法线与曲面法向相同
	- TopAbs_REVERSED 面法线和曲面法向相反
-  一个正确描述的实体，所有面都是向外的
```
flag:
	TopoDS_TEdge============Curve3d=============pCurve
						   |
						   |
						   |
						   |  Orientation 
						   |
						   |   
						   |             
						TopoDS_Edge
pCurve的走向等同于Curve3d的走向，TopoDS_TEdge的方向就是Curve的方向。
pCurve的走向等同于Curve3d的走向的一个好处是： 求交的参数曲线天然和Curve3d同向，这样节省了翻转							
可以把TopoDS_TEdge理解成Edge，TopoDS_Edge理解成HalfEdge,OCCT这么做的一个目的是为了实现共享
   
```


### 拓扑类型
-   线框(wire)包含边(edge)
-   壳(shell)包含面(face)
-   体(solid)包含壳体(shell)
-   组合体(compsolid)包含共面的体(solid)
-   复合体(compound)可以包含任意类型(包括组合体)

### 深入位置与朝向
`location`:  定义了底层几何体对于该位置的位移大小, 也定义了相对于其他子节点该节点的移动大小
`orientation`: 子节点从 实体（shape） 中分离出来， 父节点为影响子节点的朝向。例外：面中包含的边不遵循这个规律, 使用边的参数曲线
>https://zhuanlan.zhihu.com/p/437332699
- 访问参数曲线需要的操作
```cpp
TopExp_Explorer aFaceExp (myFace.Oriented (TopAbs_FORWARD), TopAbs_EDGE);
for (; aFaceExp.More(); aFaceExp.Next()) {
    const TopoDS_Edge& anEdge = TopoDS::Edge (aFaceExp.Current());
}
```
- 一个例外的例子：
 ![](https://pic1.zhimg.com/80/v2-4ecfc143d74cf2eec7260d8469403258_720w.webp)
```cpp
Handle(Geom_Surface) aSurf = new Geom_Plane(gp::XOY())

// 眼曲面的法线方向为逆时针方向的圆
Handle(Geom_Curve) anExtC = new Geom_Circle(gp::XOY(),10.); // 内圆
Handle(Geom_Curve) anIntC = new Geom_Circle(gp::XOY(),5.); // makeedge 外圆

TopoDS_Edge anExtE = BRepBuuilderAPI_MakeEdge (anExtC); // 内圆
TopoDS_Edge anIntE = BRepBuilderAPI_MakeEdge(anIntc); // 外圆 edge
TopoDS_Wire anExtW = BRepBuilderAPI_MakeWire(anExE); // 内圆环
TopoDS_Wire anIntW = BRepBuilderAPI_MakeWire(anIntE);
// 外圆环

BRep_Builder aB;
TopoDS_Face aFace;
aB.MakeFace(aFace, aSurf, Preciscion::Confusion());
aB.Update(aFace, aSurf); // 添加曲面(作画surface)
aB.Add(aFace, anExtW); // 添加外圆
aB.Add(aFace, anIntW.Reversed()); // 内圆环右侧
```
面默认情况 `forwad-orientation` ， 遍历面的所有边和参数曲线
```cpp
void TraversePCurves(const TopoDS_Face& theFace)
{
	TopExp_Exploer anExp (theFace, TopAbs_EDGE);
	while(anExp.More()){
		const TopoDS_Edge& anEdge = TopoDS::Edge(anExp.Current());
		Standard_Real aF,aL;
		Handle(Geom2d_Curve) aPCurve = BRep_Tool:CurveOnSurface(anEdge, theFace, aF, aL);
		anExp.Next();
	}
}
```
翻转这个面的朝向， 在遍历所有参数曲线:
```cpp
TopoDS_Face aRFace = TopoDS::Face(aFace.REversed());
TraverPCurves(aRFace);

// 所有的边此时具有相反的朝向，材料将位于内部现款的内测和外部线框的外侧 --> 材料的方向明显错误
// 需要以下方法访问边. 与表面(surface）的法线方向没有关系
TopExp_Explorer anExp( theFace.Oriented(TopAbs_FORWARD), TopAbs_Edge);
```


## 迭代 Shape
### 1. 递归遍历整个实体(shape)树结构
```cpp
void TraverseShape (const TopoDS_Shape& theShape)
{
	TopoDS_Iterator anIt(theShape);
	for( ; anIt.More() ; anIt.Next())
	{
		const TopoDS_Shape& achild = anIt.Value();
		TraverseShape(cChild);
	}
}
```

- `TopoDS_Iterator` 迭代器的两个标志位
	1. **位置标志**： 控制查询子节点时是否考虑父节点的位置和朝向， 如果**开启（on)** ，返回的子节点被认为独立实体
	2. **朝向标志**:  如果开启， 返回的子节点的方向是父节点朝向和子节点朝向的乘积(得到正/反向)

### 2. 访问特定类型的子节点
- 访问实体(shape)中所有的边（edge)
```cpp
TopExp_Explorer anExp(theShape, TopAbs_EDGE);
for(; anExp.More(); anExp.Next()){
	const TopoDS_Edge& anEdge = TopoDS::Edge(anExp.Current());
	// do something with an edge
}
```
- `TopExp_Explorer` 额外参数，指定**跳过的父节点**类型，如：子项检索浮动边(float edge, 不属于任何面的边)
```cpp
TopoExp_Explorrer aFloatingEdgeExp(theShape, TopAbs_EDGE, TopAbs_FACE);
```


## 转换器

一个child shape 可以同属于多个 parent shape， 例如：任何 shared edge 至少 属于两个面

### 从一个子实体追溯到父实体
```cpp
TopTools_IndexedDataMapOfShapeListOfShape anEFsMap;
// function
TopExp::MapSapesAndAncestors(myShape, TopAbs_EDGE,TopAbs_FACE, anEFsMap);
```
- `TopExp::MapSapesAndAncestors()` 这个方法为 myShape 中的面与边之间建立了映射。
- 如果myShape是一个实心长方体，每条边将映射到两个面上。如果你遍历同样的长方体，并试图找到每个面的父节点，那么很明显该映射中每个边只有一个面--也就是你当前搜索的面。


## 适配器
- 使用 AdaptorXX_XXX API
例如GeomAdaptor3d_Curve是Adaptor3d_Curve的子类，该类可以适配Geom_Curve类型，BRepAdaptor_Curve可以适配TopoDS_Edge，BRepAdaptor_CompCurve可以适配TopoDS_Wire。对于二维曲线和表面也有类似的类。
- 计算曲线或边的长度
```cpp
Handle(Geom_Curve) = ...;
Standard_Real aCurveLen = GCPoints_AbscissaPoints:Length(GeomAdaptor_Curve(aCurve));

TopoDS_Edge anEdge = ...;
Standard_Real anEdgeLen = GCPoints_AbscissaPoints:Length(BrepAdaptor_Curve(anEdge));

```