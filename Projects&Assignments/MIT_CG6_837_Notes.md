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

---
## A0. Iterated Function Systems
---
(补档)

### 原理
1. 概述
  
    6.837的第一个作业，实现IFS算法。目的是熟悉C++的语法特性，并且熟悉使用两个和图像生成和线性代数相关的简单库。通过生成一些奇妙的分形物体来体会图形学的乐趣。

    IFS **是一种构造分形的方法，得到的分形通常是自相似(self-similar)的**。IFS最经典的例子是绘制一种蕨类植物(Barnsley’s fern)，如下图所示，可以看到，每一个子叶片与整个叶片的形状相同。

    IFS由**一个仿射变换(affine transformations)集合所定义，通常定义的仿射变换包括旋转(rotation)、缩放(scale)、平移(translations)、斜切(skew)等等线性变换方法**。这些变换不着了自相似的物体形状。IFS可以定义在多个维度上，在这个作业中，我们只需实现二维的变换。


## A1. Ray Casting
---
(补档)
### 原理
1. 概述
   
    本实验的目标是要完成一个光线投射算法。光线投射算法的原理如下图（文字部分为个人见解）：

    对于每一个像素
    - 构建一条从视点处开始的射线
      - 对场景中的每一个对象
      - 判断当前射线是否和当前对象有交点（此处要用到求交点函数） 
      - 如果有且该交点距离视点更近，则更新最近交点值


## A2. Transformations
---
(补档)
### 原理
1. 概述
   
    本实验的目标是实现更多的基本图元（如Plane、Triangle类的实现）、新增两种渲染模式(normal visualization和diffuse shading)、新增PerspectiveCamera类及变换(Transform类)的实现。


## A3. Phong_Shading
---
### 原理

1. **光照模型**
   
    需要实现Blinn_Torrance版本的phong模型。

    ![](../pics/blinntorrance.png)

    公式：

    $\mathbf{h}=\frac{\mathbf{I}+\mathbf{v}}{\|\mathbf{I}+\mathbf{v}\|}$ （这一步实际上就是标准化Normalize，因此可以直接调用函数，不必用Devide）

    $L_{o}=(k_{d}(\mathbf{n} \cdot \mathbf{l})+k_{s}(\mathbf{n} \cdot \mathbf{h})^{q}) \frac{L_{i}}{r^{2}}$

    由于assignment3中使用的不是点光源，因此可以忽略r<sup>2</sup>项。
   
2. **Phong和Bling-Phong的区别**
   1. Phong和Blinn-Phong是计算镜面反射光（也叫高光反射）的两种光照模型，两者仅仅有很小的不同之处。
   2. Phong模型中计算反射光线的向量是一件相对比较耗时的任务，因此Blinn-Phong对这一点进行了改进。
   3. Phong适合模拟塑料，比"反射"材质表现出的介质更光滑一些，适合模拟玻璃、水、冰等高反光特性的介质
   4. BlinnPhong大多适用于金属材质。

3. **Flat着色和Gouraud着色**
    1. Flat着色：对于每个mesh小平面，其法向量就是该平面的向量（因此很方便计算）:
    ```cpp
      Vec3f::Cross3(n, position[index[0]] - position[index[1]], position[index[2]] - position[index[1]]);
      glNormal3f(n.x(), n.y(), n.z());
    ```
    效果就比较拉了：

    ![](../pics/non-gouraud.jpg)

    2. Gouraud着色：法线是每一步长计算出来的球法向量：
    ```cpp
      Vec3f pos = Vec3f(sin(phi)*cos(theta),cos(phi), sin(phi)*sin(theta));
			//pos.show();
			position.push_back(_center + pos * _radius);
			normal.push_back(pos); 
      ......
      glNormal3f(normal[index[i]].x(), normal[index[i]].y(), normal[index[i]].z());
    ```

    效果更好，但是计算量也更大（估计主要在三角函数的计算上）

    ![](../pics/gouraud.jpg)





### 记录点

1. **OpenGL安装**

    安装openGL时就碰到问题了，估计是dll文件一直没法链接上去，卡了半天。。

    [OpenGL+VS2017 环境配置(亲测好使)::附带必要知识点](https://blog.csdn.net/AvatarForTest/article/details/79199807?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link)

    - OpenGL只提供声明不提供实现，实现由各大硬件厂商完成
    - 库文件(lib)和头文件/包含文件(include)是相辅相成的

    .lib, .dll这些文件的区别：
    [关于lib文件的介绍](https://blog.csdn.net/m0_37876745/article/details/78323848?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.no_search_link&spm=1001.2101.3001.4242.1)

    >dll其实就是exe，只不过它没有main函数，所以不能单独执行而已。

    最后还是参考了fuzhanzhan大神的代码，直接用了他的GL文件，只要在项目中设置好include和lib的路径就好了（记得lib还得加上glut32.lib，电脑里没有就去网上下一个）


3. **球坐标换算**

    ![](../pics/meshsph1.png)

4. **使用extern时碰到的问题**

    在处理对球体进行网格化的参数的时候，本来想让其全局化，但是使用extern的时候一直在报“无法解析的外部符号”的错误。要不然只能通过传参的方式一步一步传到sphere的paint()函数中。。

    但这样就会造成raytracer的参数表太长太长，而且这个参数只有sphere类会用到，显得很冗余，不干净利落。

    最后发现问题不是在于extern，而是在我对这些参数进行具体定义的地方（main.cpp）:我把参数定义写在了main()函数里，可能正因如此它无法作为“全局”参数而extern被识别到？

    后来把参数定义部分挪出main函数后，就可以正常编译项目了。

5. **使用vector时碰到的问题**

    在Plane:print()函数中，我用vector<Vec3f>来存四个点，但是不知为何无法正确存入数据；而用类数组Vec3f Rect[4]={A,B,C,D}则可以正常运行。

6. **生成TGA图像时碰到的问题**

    第九张图片不知为何无法生成tga图像，但是openGL是正常的。

## A4. Shadows, Reflection & Refraction
---

### 原理

1. Ray Casting 和 Rendering Pipeline的区别
   
   我的理解是，这是两种计算模式？

   - Ray Casting：
     ```python
     for each pixel:
         for each object:
     ```
   - Rendering Pipeline:
     ```python
     for each triangle:
         for each pixel:
     ```

    ![](../pics/raycastingrenderpipeline.jpg)

    ![](../pics/rcandrp2.jpg)

2. Shadows
   
   先考虑引入了Phong Shading的Ray Casting：
   
   ```python
   #当你已经找到了最近的hit point时
   color = ambient*hit->getMaterial()->getDiffuseColor()
   for every light
        #对于每个光源，都分别计算并累加Shade颜色
        color += hit->getMaterial()->Shade(ray,hit,direction,lightColor)
    return color
   ```
   复杂度：$O(n*m*l)$,其中l=num of lights

   那么如何添加阴影呢？

    ```python
   #当你已经找到了最近的hit point时
   color = ambient*hit->getMaterial()->getDiffuseColor()
   # 对于每个光源
   for every light
        // ========CHANGE========
        #初始化一个从Hit点到光源的射线
        Ray ray2(hitPoint,directionToLight)
        #初始化一个t等于hitpoint到光源的距离的Hit
        Hit hit2(distanceToLight, NULL, NULL)
        #对于在场的所有物体（实际上只关注ray2光路上的物体）
        for every object:
            #迭代intersect
            object->intersect(ray2,hit2,0)
        #如果迭代之后t仍然还等于distanceToLight
        #也就是ray2光路上没有其他的阻碍物
        #这时候才更新Shade，否则就只留有ambient
        #（因为从光源到这个hitpoint之间有东西挡着）
        if(hit2->getT()=distanceToLight):
        // ========CHANGE========
            color += hit->getMaterial()->Shade(ray,hit,direction,lightColor)
    return color
   ```
   
   配图：注意橙色箭头和太阳之间有个物体阻挡。

   ![](../pics/shadow1.jpg)

   问题：自阴影（self-shadowing）--需要在计算intersect的时候添加一个epsilon（估计是增加容错率？）

   **这个epsilon现在还没搞懂是啥**

   ![](../pics/shadow2.jpg)

   **Shadow Optimization**如何提高shadow的计算效率呢？在检测ray2光路的交点时，只需要知道是否有其他交点，而不需要知道哪个交点最近

   这就需要写一个新的routine：``Object3D::intersectShadowRay()``
   and it stops at first intersection.

3. Mirror Reflection
   
   最基本的：入射角等于反射角。
   $$
   R=V-2(V·N)N
   $$

   ![](../pics/reflection1.jpg)

   


   



**记录点1：**



**记录点2：**


**记录点3：**


**记录点4：**



**记录点5：**




## A5. Voxel Rendering

**记录点1：**



**记录点2：**


**记录点3：**



**记录点4：**




## A6. Grid Acceleration & Solid Textures

**记录点1：**




## A7. Supersampling and Antialiasing

**记录点1：**




## A8. Curves & Surfaces

**记录点1：**




## A9. Particle Systems

**记录点1：**



## END
