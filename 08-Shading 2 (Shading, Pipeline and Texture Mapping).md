## Specular Term ( Blinn-Phong ) 镜面反射下

Intensity depends on view direction 强度取决于视图方向

- Bright near mirror reflection direction

  ![1679986171307](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679986171307.png)

V close to mirror direction <=> **half vector ** near normal 

- Measure "near" by dot product of unit vectors

  ![1679986362497](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679986362497.png)

  半程向量**h**为光照方向**l**和视线方向**v**之和

  半程向量**h**和法线向量**n**的夹角即光线反射方向**R**和视线方向**v**的夹角（使用**R**的话，反射向量的计算量较大）

  是否可以看到高光，跟**n**和**h**的夹角α有关，α越小，越能看到高光

  通常高光我们认为是白的，所以反射系数 k~s~ 我们给它一个白色的向量值

  Cosine Power Plots

  Increasing p narrows the reflection lobe

  ![1679988127207](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679988127207.png)

  指数p，高光系数，用来控制高光有多大，在Blinn-Phong模型中指数p一般用到100~200之间

  ![1679988500806](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679988500806.png)

## Amblent Term 环境光照下（大胆假设，实际上不是这么回事，涉及到了全局光照）

shading that does not depend on anything 不依赖于任何内容的着色

- Add constant color to account for disregarded illumination and fill in black shadows 添加恒定颜色以考虑忽略的照明并填充黑色阴影
- This is approximate / fake! 这是近似的/假的！

![1679991238101](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679991238101.png)

I~a~ : 来自环境的光的强度

k~a~ : 环境光的系数

跟实际光照方向**l**、视线方向**v**、法线向量**n**无任何关系 —— 环境光是个常数

## Blinn-Phong Reflection Model

![1679993334620](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679993334620.png)

## Shading Frequencies 着色频率

what caused the shading difference?

![1679993705715](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679993705715.png)

从球的边界可以看出，这三个球拥有完全相同的几何形状，它们的几何表示在空间中是一模一样的

这个着色，我们应该把它应用在哪些点上？

第一个球：将着色应用在每个面上，每个面都有一个法线，求出shading的结果，这个面都是这个颜色，每个面只用做一次shading

第二个球：将着色应用在每个顶点上，每个面都有三个顶点，算出每个顶点对应的法线，每个顶点做一次着色，三角形面内部的颜色通过插值的方法算出来

第三个球：将着色应用在每个像素上，对于每个三角形的顶点求出一个法线，然后把这些法线的方向在三角形内部进行插值，得到任何一个像素都有一个自己的法线方向，对每个像素做一边着色

### Shade each triangle ( flat shading ) 对每个三角形进行着色（平面着色）

**Flat** shading 平面着色

- Triangle face is flat - one normal vector 三角形平面-一个法向量
- Not good for smooth surfaces 不利于光滑表面

![1679994918011](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679994918011.png)

### Shade each vertex ( Gouraud shading ) 对每个顶点进行着色（高洛德着色）

**Gouraud** shading 高洛德着色

- **Interpolate** colors from vertices across triangle **从三角形的顶点插入**颜色
- Each vertex has a normal vector ( how? ) 每个顶点都有一个法向量（怎么做？）

![1679995279891](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679995279891.png)

### Shade each pixel ( Phong shading ) 对每个像素着色（冯着色）

**Phong** shading

- Interpolate normal vectors across each triangle
- Computer full shading model at each pixel
- Not the **Blinn-Phong Reflectance Model** 

![1679995654137](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679995654137.png)

## Shading Frequency: Face, Vertex, or Pixel

![1679995912607](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679995912607.png)\

当模型足够复杂时，要对应着去考虑选择着色频率

## Defining Per-Vertex Normal Vectors 定义逐顶点法线向量

Best to get vertex normals from the underlying geometry 从基础几何体获取顶点法线的最佳方式

- e.g. consider a sphere 考虑一个球体

Otherwise have to infer vertex normals from triangle  faces 否则必须从三角形面推断顶点法线

- Simple scheme: **average surrounding face normals** 简单方案：**平均周围面法线** 

  ![1680069812147](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680069812147.png)

![1680069795692](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680069795692.png)

- 任何一个顶点都会和不同的三角形相关联，上图四个三角形共用一个顶点，就认为顶点的法线是相邻面的法线求个平均（可能做加权平均，权可能是三角形的面积）

## Defining Per-Pixel Normal Vectors 定义逐像素法线向量

**Barycentnc interpolation ( introducing soon )** of vertex normals **顶点法线的Barycentnc插值（重心坐标插值）** 

![1680070370442](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680070370442.png)

Don't forget to **normalize** the interpolated directions 不要忘记对插值方向进行**归一化** 

# Graphics ( Real-time Rendering ) Pipeline 图形（实时渲染）管线

## Graphics Pipeline

如何从场景到最后一张图，中间到底经历了什么样的过程，这个过程称为Pipeline，即一系列不同的操作

![1680072721815](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680072721815.png)

这一系列操作都在GPU中进行

### Model, View, Projection transforms

![1680072981295](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680072981295.png)

### Sampling triangle coverage

![1680073281669](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680073281669.png)

### Z-Buffer Visibility Tests

![1680154748399](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680154748399.png)

### Shading

若是 Gouraud shading 则在 Vertex Processing 期间执行

若是 Phong shading 则在 Fragment Processing 期间执行

![1680154798848](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680154798848.png)

### Texture mapping

![1680155182721](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1680155182721.png)

渲染管线中有两部分可以用编程进行自定义 -- Shader Program

## Shader Programs

- Program vertex and fragment processing stages 程序顶点和片段处理阶段
- Describe operation on a single vertex ( or fragment )  描述对单个顶点（或片段）的操作

Example GLSL fragment shader program

```glsl
uniform sampler2D myTexture; // 全局变量 纹理
uniform vec3 lightDir; // 光照方向 每个像素有固定的光照方向 不会改变
varying vec2 uv; // 插值变量
varying vec3 norm; // 顶点法线 插值出来的

void diffuseShader(){
    vec3 kd; // 漫反射系数
    kd = texture2d(myTexture, uv); 
    // clamp()将值限制到0到1之间  dot()点乘
    kd *= clamp(dot(-lightDir, norm),0.0,1.0); // 这里光照入射方向向内 Phong模型漫反射
    gl_FragColor = vec4(kd,1.0)
}
```

可以在硬件上执行的程序

openGL 是图形学的一个API

- Shader function executes once per fragment

  shader 每一个顶点或者是每一个fragment或者说像素都会执行一次，是通用的，只用管一个顶点或一个像素怎样运作就行了

- Outputs color of surface at the current fragment's screen sample position

  fragment shader像素/片段/片元着色器的作用就是计算并输出当前片段屏幕样本位置的表面颜色

- This shader performs a texture lookup to obtain the surface's material color at this point, then performs a diffuse lighting calculation

  此着色器执行纹理查找以获得此时曲面的材质颜色，然后执行漫反射照明计算

## Snail Shader Program

<https://www.shadertoy.com/view/ld3Gz2>

隐式的几何形体，没有用任何三角形，通过数学方法定义的几何形体

## Graphics Pipeline Implementation:GPUs

用于执行图形管道计算的专用处理器（硬件层面）

- Discrete GPU Card(NVIDIA GeForce Titan X)独立显卡
- Integrated GPU(Part of Inter CPU die)集成显卡

不同类型的着色器：

- geometry shader 定义几何操作，动态产生更多的三角形
- compute shader 进行任何形式的计算，通用GPU计算（general purpose）
- directX12等在传统的基础上有所加强

## GPU:Heterogeneous,Multi-Core Procesor 异构、多核处理器

并行高性能计算

# Texture Mapping 纹理映射

## Different Colors at Different Places?

![1682563593512](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682563593512.png)

对于这个球/地板来说，公用同一种漫反射模型，只是漫反射系数发生了改变

在物体不同位置定义不同的属性 ----- 引出纹理映射的一个最基本的思路 -> 根本作用：定义任何一个点的不同属性

## Surface are 2D

Surface lives in 3D world space

Every 3D surface point also has a place where it goes in the 2D image(**texture**).

![1682564027261](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682564027261.png)

任何三维世界的物体表面都是二维的

纹理就是一张图，这张图可以任意操作（例如拉伸，裁剪等），蒙在一个物体的表面，这个过程就叫做纹理映射（物体表面点和纹理上点一一对应的关系）

## Texture Applied to Surface 

![1682564323904](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682564323904.png)

三维空间中最基本的东西是三角形。任意三角形的顶点都能在纹理上找到对应的点

三角形顶点和纹理颜色点的对应：parameterization/参数化

## Visualization of Texture Coordinates

为每个三角形顶点指定一个纹理坐标(u,v)

![1682564818960](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682564818960.png)

uv的范围，对于一张纹理来说，不管他是什么形状的，都认为`u,v∈[0,1]` 方便处理

![1682565097827](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682565097827.png)

不同位置可以映射到相同的纹理位置上，纹理可以被多次使用

![1682565204185](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682565204185.png)

![1682565242880](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682565242880.png)

![1682565317858](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1682565317858.png)

**tilable texture**: 这种纹理左边能自然衔接到右边，设计时需要各种各样不同的算法，其中有一种算法叫`wang tiling`

## Interpolation Across Triangles Barycentric Coordinates三角形重心坐标的插值

通用来说，三角形三个顶点有各自不同的属性，把这个属性在三角形内部做一个平滑的过渡，插值之后，这个点对应的属性是多少 ----- 重心坐标





