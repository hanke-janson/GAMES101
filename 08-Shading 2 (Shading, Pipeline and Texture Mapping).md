# Specular Term ( Blinn-Phong ) 镜面反射下

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

# Amblent Term 环境光照下（大胆假设，实际上不是这么回事，涉及到了全局光照）

shading that does not depend on anything 不依赖于任何内容的着色

- Add constant color to account for disregarded illumination and fill in black shadows 添加恒定颜色以考虑忽略的照明并填充黑色阴影
- This is approximate / fake! 这是近似的/假的！

![1679991238101](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679991238101.png)

I~a~ : 来自环境的光的强度

k~a~ : 环境光的系数

跟实际光照方向**l**、视线方向**v**、法线向量**n**无任何关系 —— 环境光是个常数

# Blinn-Phong Reflection Model

![1679993334620](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679993334620.png)

# Shading Frequencies 着色频率

what caused the shading difference?

![1679993705715](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679993705715.png)

从球的边界可以看出，这三个球拥有完全相同的几何形状，它们的几何表示在空间中是一模一样的

这个着色，我们应该把它应用在哪些点上？

第一个球：将着色应用在每个面上，每个面都有一个法线，求出shading的结果，这个面都是这个颜色，每个面只用做一次shading

第二个球：将着色应用在每个顶点上，每个面都有三个顶点，算出每个顶点对应的法线，每个顶点做一次着色，三角形面内部的颜色通过插值的方法算出来

第三个球：将着色应用在每个像素上，对于每个三角形的顶点求出一个法线，然后把这些法线的方向在三角形内部进行插值，得到任何一个像素都有一个自己的法线方向，对每个像素做一边着色

## Shade each triangle ( flat shading ) 对每个三角形进行着色（平面着色）

**Flat** shading 平面着色

- Triangle face is flat - one normal vector 三角形平面-一个法向量
- Not good for smooth surfaces 不利于光滑表面

![1679994918011](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679994918011.png)

## Shade each vertex ( Gouraud shading ) 对每个顶点进行着色（高洛德着色）

**Gouraud** shading 高洛德着色

- **Interpolate** colors from vertices across triangle **从三角形的顶点插入**颜色
- Each vertex has a normal vector ( how? ) 每个顶点都有一个法向量（怎么做？）

![1679995279891](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679995279891.png)

## Shade each pixel ( Phong shading ) 对每个像素着色（冯着色）

**Phong** shading

- Interpolate normal vectors across each triangle
- Computer full shading model at each pixel
- Not the **Blinn-Phong Reflectance Model** 

![1679995654137](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679995654137.png)

# Shading Frequency: Face, Vertex, or Pixel

![1679995912607](08-Shading 2 (Shading, Pipeline and Texture Mapping).assets/1679995912607.png)\

当模型足够复杂时，要对应着去考虑选择着色频率

# Defining Per-Vertex Normal Vectors 定义逐顶点法线向量

