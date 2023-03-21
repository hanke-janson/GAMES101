# Canonical Cube to Screen

**什么是屏幕？**

- 一个像素数组
- 数组大小：resolution（分辨率）（1920*1080）
- 一个典型的光栅程序设备

**Raster == screen in German**

- Rasterize == drawing  onto the screen(往屏幕中绘制)

**Pixel(FYI,short for "picture element")**

- For now: A pixel is a little square with uniform color
- Color is a mixture of(red, green,blue) 

**Defining the screen space**

![1679367444606](05-Rasterization(Triangles).assets/1679367444606.png)

- Pixels' indices(下标) are in the form of (x,y), where both x and y are integers
- Pixels' indices are from (0,0) to (width -1, height -1)
- Pixel (x,y) is centered at (x + 0.5, y + 0.5)
- The screen covers range (0,0) to (width, height)

![1679367889050](05-Rasterization(Triangles).assets/1679367889050.png)

- Irrelevant(无关) to z

- Transform in xy plane: [-1, 1]^2^ to [0, width] x [0, height]

- Viewport transform(视口变换) matrix:

  ![1679368147384](05-Rasterization(Triangles).assets/1679368147384.png)

# Next: Rasterizing Triangles into Pixels

**Rasterization: Drawing to Raster Displays**

- Polygon Meshes

  ![1679369575310](05-Rasterization(Triangles).assets/1679369575310.png)

- Triangle Meshes

  ![1679369588211](05-Rasterization(Triangles).assets/1679369588211.png)

**Triangles - Fundamental Shape Primitives**

Why  triangles?

- 最基础的多边形
- 其他多边形都可拆解为三角形
- 独特性质
  - 一定的一个平面的
  - 内部外部的定义清晰
  - 重心坐标插值

# What Pixel Values Approximate a Trangle？

## A Simple Approach(方法): Sampling（采样）

**采样**：给你一个函数，它连续的，然后我在不同的地方，我去问，这个函数的值是多少。（将一个函数离散化的过程）

> 注：这里是利用像素的中心对屏幕空间进行采样
>
> 屏幕空间定义[0, width] x [0, height]，用像素中心进行采样，相当于是算出定义在屏幕空间上的某一个函数在不同的像素中心对应的值是多少

```c++
for(int x = 0; x < xmax; ++x){
    output[x] = f(x);
}
```

Sampling is a core idea in graphics.

We sample time(1D), area(2D), direction(2D), volume(3D)...

## Rasterization As 2D Sampling

![1679370934487](05-Rasterization(Triangles).assets/1679370934487.png)

![1679370943270](05-Rasterization(Triangles).assets/1679370943270.png)

Define Binary Function: inside(tri, x, y)

> x, y: 不一定是整数

![1679371052871](05-Rasterization(Triangles).assets/1679371052871.png)

Rasterization = Sampling A 2D Indicator Function

```c++
for(int x = 0; x < xmax; ++x){
    for(int y = 0; y < ymax; ++y){
        image[x][y] = inside(tri, x + 0.5, y + 0.5);
    }
}
```

Sample location for pixel (x, y)

![1679373361722](05-Rasterization(Triangles).assets/1679373361722.png)

叉乘判断点是否在三角形内

## Checking All Pixels on the screen?

Use a Bounding Box!    axis-aligned bounding box (AABB)(轴向包围盒) 

![1679374177938](05-Rasterization(Triangles).assets/1679374177938.png)

Incremental Triangle Traversal(Faster?)

suitable for thin and rotated triangles(适用于薄三角形和旋转三角形)

![1679374390559](05-Rasterization(Triangles).assets/1679374390559.png)

## Raterization on Real Displays

**Real LCD Screen Pixels(Closeup)**

![1679378024452](05-Rasterization(Triangles).assets/1679378024452.png)

Galaxy S5屏幕：Bayer-pattern 绿色点较多，人眼对绿色更敏感

**Aside：What About Other Display Methods?**

Color print: observe half-tone pattern

![1679378040671](05-Rasterization(Triangles).assets/1679378040671.png)

检测系统，往上加的颜色越多，结果越黑

## Assume Display Pixels Emit Square of Light

**If We Send the Display the Sampled Signal**

![1679378245362](05-Rasterization(Triangles).assets/1679378245362.png)

**The Display Physically Emits This Signal** (Aliasing(走样): Jaggiees!锯齿)

![1679378261371](05-Rasterization(Triangles).assets/1679378261371.png)

