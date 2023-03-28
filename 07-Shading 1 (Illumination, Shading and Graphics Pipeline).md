# Visibility / occlusion（可见性/遮挡）

## Painter's Algorithm

Inspired by how painters paint

Paint from back to front, **overwrite** in the framebuffer

Requires sorting in depth ( O( n log n ) for n triangles )  深度排序(快排)

Can have unresolvable depth order  可能具有无法解决的深度顺序

![1679447936991](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679447936991.png)

## Z-Buffer 深度缓存

This is the algorithm that eventually won

Idea:

会渲染最后的成品图（结果），生成这张图像的同时，也会生成另外一个图像，这个图像只存任何一个像素它所看到的几何物体的最浅的深度的信息，这个图像就叫做深度图/深度缓存，也就是说，始终同步生成两个不同的东西，一个是最后的结果(frame buffer),一个是我看到的当前的场景所对应的深度(depth buffer),这两个东西同时生成，然后我们利用这个深度缓存的这个想法来维护遮挡信息

- Store current min. z-value **for each sample(pixel)** 存每个像素最小的z值（离我们最近的一个距离，最浅的深度）
- Need an additional buffer for depth values 深度值需要额外的缓冲区
  - frame buffer stores color values
  - depth buffer ( z-buffer ) stores depth

y轴是负的 所以y轴上的值越大，离我们越近

IMPORTANT: For simplicity we suppose **z is always positive** (smaller z -> closer, larger z -> further )

重要提示：为了简单起见，我们假设**z总是正的(代表物体离摄像机的距离)**（z越小，z越近，z越大，z越远）

## Z-Buffer Example

![1679452762607](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679452762607.png)

在深度图中，离我们越近，颜色越黑

## Z-Buffer Algorithm

Initialize depth buffer to ∞（将深度缓冲初始化为∞）认为所有深度缓存中的所有像素开始记录的距离/深度无限远

During rasterization（光栅化期间）: 

```c++
for(each triangle T){ // 遍历每一个三角形
    for(each sample(x,y,z) in T){ // 找到每一个像素
		if(z < zbufffer[x,y]){ // closest sample so far 迄今为止最接近的样本（如果这个深度小于之前记录好的深度，那就把这个深度缓存的值更新成小的这个深度，并且把新的这个三角形的颜色给画进去）
		   framebuffer[x,y] = rgb; // update color
            zbuffer[x,y] = z; // update depth
        }else{
            ; // do nothing, this sample is occluded 这个样本被遮挡了，就不去更新颜色和深度缓存了
        }
    }
}
```

![1679454305993](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679454305993.png)

R - 无限大（远）c++中inf 浮点数的一个特殊值

深度缓存是发生在每一个像素内的

## Z-Buffer Complexity(复杂性)

Complexity

- O (n) for n triangles (assuming constant coverage)
- How is it possible to sort n triangles in linear time?

假设认为每个三角形都不是特别大不是特别小，覆盖一定的常数个像素，例如每个三角形都会覆盖100个左右的像素，那这个深度缓存的算法复杂度是？无非就是一个个三角形往里填，每个三角形考虑它覆盖的常数个像素，常数乘以三角形个数个操作。对于n个三角形来说，就是O (n)的一个操作。（这里没有进行排序）

这里和绘制三角形的顺序是没有关系的

Most important visibility algorithm 最重要的可见性算法

- Implemented in hardware for all GPUs 在所有GPU的硬件中实现

透明物体不能用深度测试

# Shading(着色)

**Definition**

- In Merriam-Webster Dictionary

  The darkening or coloring of an illustration or diagram with paraller lines or a block of color. 

- In this course

  对不同的物体应用不同的材质

【着色不考虑其他物体的存在，只考虑这个点自身】

## A Simple Shading model (Blinn-Phong Reflectance Model)

Perceptual Observations 感知观察

![1679899273542](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679899273542.png)

高光 漫反射 间接光照

### Shading is Local （局部）

Compute light reflected roward camera at a specific **shading point**

![1679899823293](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679899823293.png)

Inputs:（无论物体时平面还是曲面，都认为在极小的面上是一个平面）

- Viewer direction, v 观测方向
- Surface normal, n 法线
- Light direction, l ( for each of many lights ) 光照方向
- Surface parameters ( color, shininess光泽度（和亮度两码事）, ... ) 其他属性

**No shadows** will be generated( **shading** ≠ **shadow** )不会生成阴影

### Diffuse Reflection(漫反射)

- Light is scattered uniformly in all directions（光向各个方向均匀散射）
  - Surface color is the same for all viewing directions

![1679900635843](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679900635843.png)

- But how much light ( energy ) is received? 但是接收到的光（能量）是多少？

  - Lambert's cosine law    Lambert余弦定律

    ![1679900965901](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679900965901.png)

    通过上图可以发现：法线和光线方向的夹角决定了物体表面的亮度，这里考虑单位面积

  - Light Falloff 光的衰减

![1679901754535](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679901754535.png)

- Lambertian ( Diffuse ) Shading 
  - Shading **independent** of view direction 着色**与视图方向v无关** 

![1679901898869](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679901898869.png)

K~d~ ：漫反射系数，用来表示颜色波段能量的吸收，当 K~d~ = 1 时，表示这个点完全不吸收能量，多少能量进来，多少能量被反射出去，这一点是最亮的(白)；当 K~d~ = 0 时， 表示这个点吸收所有能量，多少能量进来，就吸收多少能量，这一点是最暗的(黑)。

将 K~d~ 的值设置为一个三通道的向量，即可表示颜色

max(0,n·l) : 能量接收的量， n·l可以计算出法线和光线方向的夹角的余弦值，可能为负，为负时无意义

I/r^2^ : 能量到达的量

Produces diffuse appearance 产生漫反射外观

![1679906227234](07-Shading 1 (Illumination, Shading and Graphics Pipeline).assets/1679906227234.png)

