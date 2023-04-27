# Interpolation Across Triangles: Barycentric Coordinates三角形插值：重心坐标

## Interpolation Across Triangles

为什么做插值？？

- Specify values **at vertices** 指定顶点处的值
- Obtain smoothly varying values **across triangles** 获得三角形之间平滑变化的值

我们的很多操作都是在三角形顶点上完成或者计算的，然后在三角形内部希望得到一个平滑的过渡

对什么做插值？？

- Texture coordinates, colors, normal vectors, ... 纹理坐标、颜色、法线向量...
- 基本来说，可以对三角形顶点上的任意属性进行插值

怎么做插值？？

- **Barycentric coordinates** 重心坐标

## Barycentric coordinates

A coordinate system for triangles(α, β, γ)

![1682567119392](09-Shading 3 (Texture Mapping Cont.).assets/1682567119392.png)

- 三角形所在平面上的任意一个点(x, y)都可以用**三个顶点坐标的线性组合**来表示
- **α+β+γ=1**时，这个点才会在三角形所在平面上
- 如果这个点在**三角形内**，还需要满足**α、β、γ必须是非负的** —— 重心坐标

![1682567605842](09-Shading 3 (Texture Mapping Cont.).assets/1682567605842.png)

Geometric viewpoint -- proportional areas 几何视点——比例区域

![1682574149191](09-Shading 3 (Texture Mapping Cont.).assets/1682574149191.png)

通过三角形的面积比求αβγ

质心的重心坐标是多少？

![1682574581451](09-Shading 3 (Texture Mapping Cont.).assets/1682574581451.png)

公式

![1682574710134](09-Shading 3 (Texture Mapping Cont.).assets/1682574710134.png)

## Using Barycentric Coordinates

Linearly interpolate values at vertices 对顶点处的值进行线性插值

![1682574937735](09-Shading 3 (Texture Mapping Cont.).assets/1682574937735.png)

通过αβγ将各种属性值分别线性的组合起来

**在投影变换下不能保证重心坐标不变！**

> 例：在三维空间中的三角形有个点，投影了之后，他的重心坐标可能就不一样了

想要插值一些三维空间中的属性，我们就应该取三维空间中的坐标（a, b, c）再去找三维空间中的重心坐标之后再去做插值，不能在投影之后的三角形中去做（**深度**） 

# Applyinig Textures

## Simple Texture Mapping: Diffuse Color 简单纹理贴图：漫反射颜色

![1682575889384](09-Shading 3 (Texture Mapping Cont.).assets/1682575889384.png)

1.屏幕上的采样点，插值出来的uv，或者说纹理的坐标

2.在纹理上查询一下这个uv，可以知道对应的纹理颜色

3.我们可以认为这个纹理颜色就是漫反射系数，用纹理颜色去代替漫反射系数，就相当于将这张图贴在了这个物体上，并且这个物体还有Phong-shading带来的明暗变换，高光等

## 出现的问题

### 纹理放大（如果纹理太小怎么办？）

例：有一面很大分辨率的墙，可能有4K??我们的纹理只有256*256，咋办？纹理太小了，如果要对应uv，纹理就会被拉大

Texture Magnification - Easy Case

Generally don't want this —— insufficient texture resolution A pixel on a texture 一般情况下不希望这样——纹理分辨率不足纹理上的像素—— a **texel** (纹理元素、纹素)

![1682576778030](09-Shading 3 (Texture Mapping Cont.).assets/1682576778030.png)

对于任何的墙上的一个点/像素，都可以找到它对应的这个纹理上的一个位置，这个位置可能不是整数，咱们就把它around四舍五入成整数 —— 相当于在一定范围内，查找相同的纹理上的像素，叫texel，纹素

当在查询这个纹理的时候，如果查找到一个非整数的坐标，那应该如何得到它的值？ —— **双线性插值** 

**Bilinear Interpolation** 

![1682577299994](09-Shading 3 (Texture Mapping Cont.).assets/1682577299994.png)

![1682577558677](09-Shading 3 (Texture Mapping Cont.).assets/1682577558677.png)

![1682577586169](09-Shading 3 (Texture Mapping Cont.).assets/1682577586169.png)

![1682577865652](09-Shading 3 (Texture Mapping Cont.).assets/1682577865652.png)

**Bicubic Interpolation**  双三次插值

取这个点周围临近的16个点对竖直和水平分量进行插值计算

### 纹理放大（如果纹理太大怎么办？）

Texture Magnification - Hard Case

Point Sampling Textures -- Problem

![1682579797437](09-Shading 3 (Texture Mapping Cont.).assets/1682579797437.png)

会产生走样的问题

Screen Pixel "Footprint" in Texture

![1682579994450](09-Shading 3 (Texture Mapping Cont.).assets/1682579994450.png)

Will Supersampling Do Antialiasing?

![1682580136130](09-Shading 3 (Texture Mapping Cont.).assets/1682580136130.png)

Antialiasing —— Supersampling?

Will supersampling work?  超采样有效吗？

- Yes, high quality, but costly 质量高，但算法很慢
- When highly minified, many texels in pixel footprint 当高度缩小时，像素占地面积中有许多纹素
- Signal frequency too large in a pixel  像素中的信号频率过大
- Need even higher sampling frequency 需要更高的采样频率

避免采样 —— 给一个区域，立刻知道这个区域的平均值 —— **mipmap**

**点查询问题和范围查询问题**

- 点查询( Point Query )：以texture纹理为例的双线性插值
- 范围查询( Range Query )：mipmap

## Mipmap - Allowing(fast, approx, square)range query

特点：快，近似(不准确)，只能做正方形区域的范围查询

Mipmap(L. Williams 83)

![1682581465569](09-Shading 3 (Texture Mapping Cont.).assets/1682581465569.png)

从一张图生成一系列图

计算机视觉中叫image pyramid

![1682582209387](09-Shading 3 (Texture Mapping Cont.).assets/1682582209387.png)

mipmap的存储开销是多少？

存储量为4/3, 额外的存储开销为原来的1/3

### Computing Mipmap Level D

![1682582769036](09-Shading 3 (Texture Mapping Cont.).assets/1682582769036.png)

![1682582983234](09-Shading 3 (Texture Mapping Cont.).assets/1682582983234.png)

![1682583877189](09-Shading 3 (Texture Mapping Cont.).assets/1682583877189.png)

用这个正方形框来近似这个不规则的映射区域，即通过在像素上移动多少距离，计算出在纹理上移动多少距离

根据之前计算好的mipmap查询边长为L的这个区域的值的平均值是多少

如果这个区域大小就是1\*1，就是一个像素的话，就可以在最原始的没有做过mipmap的这个纹理上找到对应的像素；

如果这个区域大小是4\*4， 那么这个区域在第二层上一定会变成1个像素

> 原始纹理图片为0层，对应区域为4\*4，那么j经过mipmap后第一层对应的区域为2\*2，第二层对应的区域为1\*1

Visualization of Mipmap Level 

![1682584926634](09-Shading 3 (Texture Mapping Cont.).assets/1682584926634.png)

发现问题：不连续，因为咱们只mipmap了0,1,2...层，没有计算1.8层这种....

解决：利用插值查找1.8层

> 先寻找1层和二层，然后在这两层内部，分别使用双线性插值把在这两层上对应的查询做出来，然后将两次双线性插值的结果合在一块，就可以在层与层之间再进行一次插值

**Trilinear Interpolation 三线性插值** 

![1682585277245](09-Shading 3 (Texture Mapping Cont.).assets/1682585277245.png)

![1682585667294](09-Shading 3 (Texture Mapping Cont.).assets/1682585667294.png)

mipmap的问题

![1682585786565](09-Shading 3 (Texture Mapping Cont.).assets/1682585786565.png)

![1682585797482](09-Shading 3 (Texture Mapping Cont.).assets/1682585797482.png)

Overblur 过度模糊

> 原因：近似正方形区域内查询

## Anisotropic Filtering 各向异性过滤

![1682585978580](09-Shading 3 (Texture Mapping Cont.).assets/1682585978580.png)

**Ripmaps and summed area tables**

- Can look up **axis-aligned rectangular zones** 可以查找**轴对齐的矩形区域**
- Diagonal footprints still a problem 对角线足迹仍然是个问题

![1682586180063](09-Shading 3 (Texture Mapping Cont.).assets/1682586180063.png)

水平竖直压扁，这样可以查询到对应原始图上的矩形区域，就不会限制到一个正方形的区域上

各向异性过滤的额外开销是原来的三倍

Irregular Pixel Footprint in Texture 纹理中的不规则像素足迹

![1682586367776](09-Shading 3 (Texture Mapping Cont.).assets/1682586367776.png)

屏幕上的像素映射到纹理上时，不一定都是一个规律的形状，很有可能出现这种超级斜着的细的区域

如果引入各向异性过滤，就可以解决这种长条形区域的问题了，但是只是解决了一部分的问题，对于斜着的区域还是有些问题

**EWA filtering**

- Use multiple lookups 使用多个查找
- Weighted average 加权平均
- Mipmap hierarchy still helps Mipmap层次结构仍然有帮助
- Can handle irregular footprints 可以处理不规则的脚印

![1682586931053](09-Shading 3 (Texture Mapping Cont.).assets/1682586931053.png)

有一个任意不规则的形状，它都可以把它拆成很多不同的圆形去覆盖这个不规则形状





