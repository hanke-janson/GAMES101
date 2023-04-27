# Many, Many Uses for Texturing

In modern GPUs, texture = memory + range query ( filtering )

- General method to bring data to fragment calculations 

Many applications

- Environment lighting
- Store microgeometry
- Procedural textures
- Solid modeling
- Volume rendering
- ...

Environment Map

犹他茶壶

![1682588820203](10-Geometry 1 (Introduction).assets/1682588820203.png)

认为环境光都来自无限远处，假设只记录一个光照方向信息就可以了

Environment lighting

![1682589210486](10-Geometry 1 (Introduction).assets/1682589210486.png)

Spherical Environment Map

![1682589302251](10-Geometry 1 (Introduction).assets/1682589302251.png)

Spherical Map —— Problem

![1682589427944](10-Geometry 1 (Introduction).assets/1682589427944.png)

Cube Map

![1682589601502](10-Geometry 1 (Introduction).assets/1682589601502.png)

A vector maps to cube point along that direction.向量沿着该方向映射到立方体点

The cube is textured with 6 square texture maps.立方体使用6个正方形纹理贴图进行纹理处理

