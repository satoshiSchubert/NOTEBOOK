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
  - [3. Phong_Shaing](#3-phong_shaing)
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

产生 num 个随机点，经过 iters 次矩阵（随机选取）变换，保存到图片中对应位置。 



### 1. Ray Casting

实现 Algebraic 和 Geometric 两种方法计算球体交点。

**注意点1：**

不要 delete 同一片内存两次，不需要在 ~Object3D() 中 delete material; 因为 material 申请内存的操作由  ~SceneParser() 负责。


**注意点2：**

image 对应于（0, 200）和比例（0, 1）。

OrthographicCamera 的 center 在（0, 0），因此产生的光线对应于 （-1, 1）* size。

**注意点3：**

error: multiple definition of 函数多次定义，当某些辅助函数作为全局函数且在 .h 文件中时会发生。

办法一：把这些函数定义在 .cpp 文件中，但同名函数也只能定义一份。

办法二：把这些函数作为类内函数。

**注意点4：**

在使用 Geometric 法计算交点时，为了得到与样例相符的图片，忽略 t 是否在视线后面，即 camera center 是否在球体内部，永远取 t = min(t1, t2)。



### 2. Transformations

**注意点1：**

三角形计算交点可以使用 Matrix.cpp 中的 det3x3。

**注意点2：**

把 rd 从 world-space 转换到 object-space 的时候不要 normalize()。

**注意点3：**

scene_16


### 3. Phong_Shaing

**注意点1：**

Phong 与 BlinnPhong 的区别在于，前者在 v, l 同向时且 exponent 较小的时候，会产生明显的断层现象。

**注意点2：**

OpenGL 画球体中，phi_steps 作为纬度的步长，取到首尾， theta_steps 作为经度的步长，取首不取尾。把球体定义为 quad，比定义为 triangle 简单。

**注意点3：**

Flat shading (visible facets)  使用 quad 的 normal，quad 是一个平面，因此产生可见面。




### 4. Shadows, Reflection & Refraction

**注意点1：**

正交相机判断阴影的方法，设 tmin = 0.0001f; 而不是 getTmin() + 0.0001f;

**注意点2：**

使用 OpenGL previsualization，能够找出大部分关于反射和折射的问题，在 scene4_07 8 9 涉及球体的内部反射折射问题，只能使用 Algebraic 算法来求交点，Geometric 法求交会减少射线的产生，发生错误。

**注意点3：**

RayTracer::traceRay(Ray &ray, float tmin, int bounces, float weight, Hit &hit);

**注意点4：**



**注意点5：**

tmin 这个数据其实可以设置在 hit 类中，但由于延续课程的风格，就放在函数参数中了。



### 5. Voxel Rendering

**注意点1：**



**注意点2：**


**注意点3：**



**注意点4：**




### 6. Grid Acceleration & Solid Textures

**注意点1：**




### 7. Supersampling and Antialiasing

**注意点1：**

 Gaussian filter 中，令  sigma  = radius，为了得到正确的结果，取消 d > 2 * sigma 的判断。

**注意点2：**

Filter::getColor 中，要计算所有在 getSupportRadius() 范围内的像素的采样，另外要求总 weight = 1。

**注意点3：**

渲染 scene7_03_marble_vase.txt 的最后一幅图时，在 Sampler类中，若 d = 1.0f / (size + 1);  会触发





### 8. Curves & Surfaces

**注意点1：**

 *B* 矩阵可以作为 static 变量写在 Spline 类中。

**注意点2：**

注意 TriangleNet 的初始化和顶点的设置。

**注意点3：**



### 9. Particle Systems

**注意点1：**



### END
