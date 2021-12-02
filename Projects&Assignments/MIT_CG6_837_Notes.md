---
title: MIT-CG-6.837--2004
date: 2021-11-26 21:45:00
tags:
- Assignments
categories:
- Projects&Assignments
---

<p align='center'>
<a href="https://github.com/satoshiSchubert" target="_blank"><img src="https://img.shields.io/badge/Github-@SatoshiNg-f3e1e1.svg?style=flat-square&logo=GitHub"></a>
<img src='https://img.shields.io/badge/style-Chinese-c45a65.svg?style=flat-square' />

# MIT-CG-6.837--2004
---
**6.837 Intro to Computer Graphics, Fall 2004**

http://groups.csail.mit.edu/graphics/classes/6.837/F04/index.html 

环境配置：

- Win10 + VS2017
- source: 源代码文件
- release: 可执行文件，input file和批处理命令

## 目录
<!-- TOC -->

- [# MIT-CG-6.837--2004](#-mit-cg-6837--2004)
- [目录](#目录)
  - [0. Iterated Function Systems](#0-iterated-function-systems)
  - [1. Ray Casting](#1-ray-casting)
  - [2. Transformations](#2-transformations)
  - [3. Phong_Shading](#3-phong_shading)
  - [4. Shadows, Reflection & Refraction](#4-shadows-reflection--refraction)
  - [5. Voxel Rendering](#5-voxel-rendering)
  - [6. Grid Acceleration & Solid Textures](#6-grid-acceleration--solid-textures)
  - [7. Supersampling and Antialiasing](#7-supersampling-and-antialiasing)
  - [8. Curves & Surfaces](#8-curves--surfaces)
  - [9. Particle Systems](#9-particle-systems)
  - [END](#end)

<!-- /TOC -->
---
### 0. Iterated Function Systems



### 1. Ray Casting




### 2. Transformations



### 3. Phong_Shading

**记录点1：**

安装openGL时就碰到问题了，估计是dll文件一直没法链接上去，卡了半天。。

[OpenGL+VS2017 环境配置(亲测好使)::附带必要知识点](https://blog.csdn.net/AvatarForTest/article/details/79199807?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link)

- OpenGL只提供声明不提供实现，实现由各大硬件厂商完成
- 库文件(lib)和头文件/包含文件(include)是相辅相成的

.lib, .dll这些文件的区别：
[关于lib文件的介绍](https://blog.csdn.net/m0_37876745/article/details/78323848?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.no_search_link&spm=1001.2101.3001.4242.1)

>dll其实就是exe，只不过它没有main函数，所以不能单独执行而已。

最后还是参考了fuzhanzhan大神的代码，直接用了他的GL文件，只要在项目中设置好include和lib的路径就好了（记得lib还得加上glut32.lib，电脑里没有就去网上下一个）

**记录点2：**

需要实现Blinn_Torrance版本的phong模型。

![](../pics/blinntorrance.png)

公式：

$\mathbf{h}=\frac{\mathbf{I}+\mathbf{v}}{\|\mathbf{I}+\mathbf{v}\|}$

$L_{o}=(k_{d}(\mathbf{n} \cdot \mathbf{l})+k_{s}(\mathbf{n} \cdot \mathbf{h})^{q}) \frac{L_{i}}{r^{2}}$

由于assignment3中使用的不是点光源，因此可以忽略r<sup>2</sup>项。

**记录点3：**



### 4. Shadows, Reflection & Refraction

**记录点1：**



**记录点2：**


**记录点3：**


**记录点4：**



**记录点5：**




### 5. Voxel Rendering

**记录点1：**



**记录点2：**


**记录点3：**



**记录点4：**




### 6. Grid Acceleration & Solid Textures

**记录点1：**




### 7. Supersampling and Antialiasing

**记录点1：**




### 8. Curves & Surfaces

**记录点1：**




### 9. Particle Systems

**记录点1：**



### END
