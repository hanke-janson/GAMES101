# ReCall

![1679379312569](06-Rasterization(Antialiasing and Z-Buffering).assets/1679379312569.png)

![1679379319859](06-Rasterization(Antialiasing and Z-Buffering).assets/1679379319859.png)

![1679379329840](06-Rasterization(Antialiasing and Z-Buffering).assets/1679379329840.png)

Jaggies!

# Aliasing(走样)

Sampling **Artifacts(瑕疵)** (Errors / Mistakes / Inaccuracies) in Computer Graphics

**Jaggies(锯齿)(Staircase Pattern)** - sampling in space

This is also an example of "aliasing" - a sampling error

![1679379471263](06-Rasterization(Antialiasing and Z-Buffering).assets/1679379471263.png)

**Moire*[mwa:]*(摩尔纹) Patterns in Imaging** - undersampling images

Skip odd rows and columns 

![1679379994521](06-Rasterization(Antialiasing and Z-Buffering).assets/1679379994521.png)

**Wagon Wheel Illusion (False Motion)**  - sampling in time

[Many more]...

Behind the Aliasing Artifacts

- Signals are changing too fast (high frequency), but sampled too slowly

## Antialiasing Idea: Blurring (Pre-Filtering) Before Sampling(采样前模糊（预滤波）)

Note antialiased edges in rasterized triangle where pixel values take intermediate values

![1679381349088](06-Rasterization(Antialiasing and Z-Buffering).assets/1679381349088.png)

Point Sampling

![1679381516504](06-Rasterization(Antialiasing and Z-Buffering).assets/1679381516504.png)

Antialiasing

![1679381536560](06-Rasterization(Antialiasing and Z-Buffering).assets/1679381536560.png)

Point Sampling vs Antialiasing

![1679381585287](06-Rasterization(Antialiasing and Z-Buffering).assets/1679381585287.png)xx

Blurred Aliasing vs Antialiasing

![1679381885086](06-Rasterization(Antialiasing and Z-Buffering).assets/1679381885086.png)

先采样后滤波 WRONG！！！整体锯齿变模糊了

## Frequency(频率) Domain(域)

Sines and Cosines

![1679382043308](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382043308.png)

频率 cos2Πfx

![1679382355350](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382355350.png)

Fourier Transform(傅里叶级数展开)

Represent a function as a weighted sum of sines and cosines（将函数表示为正弦和余弦的加权和）

![1679382578587](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382578587.png)

![1679382629934](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382629934.png)

![1679382834970](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382834970.png)

![1679382853812](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382853812.png)

![1679382915803](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382915803.png)

![1679382934362](06-Rasterization(Antialiasing and Z-Buffering).assets/1679382934362.png)

![1679383360943](06-Rasterization(Antialiasing and Z-Buffering).assets/1679383360943.png)

![1679383371422](06-Rasterization(Antialiasing and Z-Buffering).assets/1679383371422.png)

Fourier Transform Decomposes A Signal Into Frequencies（傅里叶变换将信号分解为频率）

![1679383564743](06-Rasterization(Antialiasing and Z-Buffering).assets/1679383564743.png)

Higher Frequencies Need Faster Sampling（更高的频率需要更快的采样）

![1679383963855](06-Rasterization(Antialiasing and Z-Buffering).assets/1679383963855.png)

走样：同样的采样方法采样两种频率的函数，得出的结果无法区分它

![1679384218586](06-Rasterization(Antialiasing and Z-Buffering).assets/1679384218586.png)

Filtering(滤波) = **Getting rid of(去除)** certain frequency contents

Visualizing Image Frequency Content

(低频信息集中在中间，高频信息集中在周围， 信息越多越亮)

![1679384655035](06-Rasterization(Antialiasing and Z-Buffering).assets/1679384655035.png)

任何信号在不同的频率都长什么样，叫**频谱**

Filter Out Low Frequencies Only(Edges)仅滤除低频（边缘）

**High-pass filter** （高通滤波）

![1679384950424](06-Rasterization(Antialiasing and Z-Buffering).assets/1679384950424.png)

Filter Out High Frequencies Only(Blur)仅滤除高频（模糊）

**Low-pass filter** （低通滤波）

![1679385355969](06-Rasterization(Antialiasing and Z-Buffering).assets/1679385355969.png)

Filter Out Low and High Frequencies滤除低频和高频

![1679385498646](06-Rasterization(Antialiasing and Z-Buffering).assets/1679385498646.png)

![1679385558974](06-Rasterization(Antialiasing and Z-Buffering).assets/1679385558974.png)

《数字图像处理》

## Filtering = Convolution ( = Averaging ) 滤波=卷积(=平均)

**Convolution** : Point-wise local averaging in a "sliding window"

![1679386094432](06-Rasterization(Antialiasing and Z-Buffering).assets/1679386094432.png)

![1679386170854](06-Rasterization(Antialiasing and Z-Buffering).assets/1679386170854.png)

Convolution Theorem（卷积定理）

![1679386412437](06-Rasterization(Antialiasing and Z-Buffering).assets/1679386412437.png)

时域的卷积=频域的乘积

Box Filter

不归一化的话，越滤波值越大越亮

![1679386628756](06-Rasterization(Antialiasing and Z-Buffering).assets/1679386628756.png)

Box Function = "Low Pass" Filter

![1679386959285](06-Rasterization(Antialiasing and Z-Buffering).assets/1679386959285.png)

Wider Filter Kernel = Lower Frequencies (较宽的滤波器内核=较低的频率)

![1679387113766](06-Rasterization(Antialiasing and Z-Buffering).assets/1679387113766.png)

## Sampling = Repeating Frequency Contents 采样=重复频率内容

![1679387482144](06-Rasterization(Antialiasing and Z-Buffering).assets/1679387482144.png)

c:冲激函数

a*c=e           b卷积d=f

采样就是重复一个原始信号的频谱

Aliasing = Mixed Frequency Contents

![1679387926179](06-Rasterization(Antialiasing and Z-Buffering).assets/1679387926179.png)

采样的不够快就会导致原始的信号，它的这个复制粘贴，中间的间隔就会非常小，就是说，采样的不够快，意味着采样点之间的距离会很大，对应到频谱的复制粘贴上，中间的间隔就会很小。

原始信号和复制粘贴的信息混叠在一起了 ---- 这种情况下就是发生了走样。

# Antialiasing(反走样，消除锯齿)

如何降低走样影响？？

- 增加采样率（受制于物理硬件）
- 反走样

Antialiasing = Limiting, then repeating 先做个模糊处理，把高频信息拿掉（将混叠部分拿掉），再进行采样

![1679389033137](06-Rasterization(Antialiasing and Z-Buffering).assets/1679389033137.png)

Antialiased Sampling

![1679389343969](06-Rasterization(Antialiasing and Z-Buffering).assets/1679389343969.png)

pre-filter：用一定大小的低通滤波器对其进行卷积

**A Practical Pre-Filter 一种使用的滤波器**

A 1 pixel-width box filter (low pass, blurring)

![1679389559551](06-Rasterization(Antialiasing and Z-Buffering).assets/1679389559551.png)

## Antialiasing By Averaging Values in Pixel Area

解决方案：

- **Convolve** f(x,y) by a 1-pixel box-blur
  - Recall: convolving = filtering = averaging
- **Then sample** at every pixel's center

## Antialiasing by Computing Average Pixel Value

In rasterizing one triangle, the average value insode a pixel area of f(x,y) = inside(triangle,x,y) is equal to the area of the pixel covered by the triangle.(在光栅化一个三角形时，f(x,y) = inside(triangle,x,y) 的像素面积内的平均值等于三角形覆盖的像素面积。)

对每个像素中原始图像的覆盖区域求覆盖面积，根据这个覆盖面积赋给它对应比例的灰值（将每个像素终点覆盖面积平均起来，即卷积操作）

![1679390031559](06-Rasterization(Antialiasing and Z-Buffering).assets/1679390031559.png)

## Antialiasing By Supersampling(MSAA)超采样反走样

用更多的采样点来进行反走样

Approximate the effect of the 1-pixel box filter by sampling mulyiple locations within a pixel and averaging their values:(通过对一个像素内的多个位置进行采样并对其值取平均值来近似1像素盒滤波器的效果)

![1679390893765](06-Rasterization(Antialiasing and Z-Buffering).assets/1679390893765.png)

Point Sampling: One Sample Per Pixel

![1679390966935](06-Rasterization(Antialiasing and Z-Buffering).assets/1679390966935.png)

**Supersampling: Step 1** （MSAA: 通过更多的样本近似的是反走样的第一步 -- 模糊的过程，为了近似一个合理的覆盖率，并没有真的提高屏幕上的这个分辨率，该是多少像素还是多少像素，解决的是对信号的模糊操作）

Take N*N samples in each pixel.

![1679391033206](06-Rasterization(Antialiasing and Z-Buffering).assets/1679391033206.png)

**Supersampling: Step 2**

Average the N*N samples "inside" each pixel.

![1679391229678](06-Rasterization(Antialiasing and Z-Buffering).assets/1679391229678.png)

![1679391274837](06-Rasterization(Antialiasing and Z-Buffering).assets/1679391274837.png)

上图中的鼠标所指的点，求其覆盖率75%

![1679391362141](06-Rasterization(Antialiasing and Z-Buffering).assets/1679391362141.png)

上图中的鼠标所指的点，求其覆盖率25%

**Supersampling: Result**

This is the corresponding signal emitted by the display

![1679391655684](06-Rasterization(Antialiasing and Z-Buffering).assets/1679391655684.png)

No free lunch!

- What's the cost of MSAA?为了使用MSAA达到抗锯齿的效果，牺牲了什么？
  - 用更多的点去测试是否在三角形内，增大了计算量，例如一个像素用了4*4的卷积核计算，那就是增加了16倍的计算量

Milestones 里程碑

- FXAA ( Fast Approximate AA ) 快速近似抗锯齿 【它是一个图像的后期处理，得到带有锯齿的图像后，直接通过图像匹配的方式找到边界，然后将边界换成没有锯齿的边界，与采样无关，很快，在图像层面做的抗锯齿】
- TAA ( Temporal AA ) 【与时间相关，找上一帧信息，复用上一帧的结果，将MSAA对应的样本给分布在时间上，当前这一帧没有引入任何操作】

Super resolution / super sampling 超分辨率/ 超采样

- 从低分辨率到高分辨率
- Essentially still "not enough samples" problem 本质上仍然存在“样本不足”问题
- DLSS ( Deep Learning Super Sampling ) 深度学习超级采样



