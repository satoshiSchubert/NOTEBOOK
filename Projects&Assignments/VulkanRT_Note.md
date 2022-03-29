# VulkanRT学习笔记

Author: Satoshi Schubert

Email: crisprhhx@outlook.com

reference：

1. <https://zhuanlan.zhihu.com/p/20712354>

2. <https://renderdoc.org/vulkan-in-30-minutes.html>

## Vulkan部分

-----

### INTRO:Vulkan的几个基本模块

1. Device：
   1. 也就是GPU
2. Pipeline：
   1. 包括大部分状态和设定，在初始化的时候就完成验证和编译。
   2. 因此如果有不同的Pass，就需要提前写好不同的Pipeline。
   3. 然而我们不能把所有渲染需要的信息全都prebake进pipeline中，一个Pipeline应该是可以通过绑定不同的资源而复用的。
3. Buffer：
   1. 不同种类的内存分配,指定cpu、gpu谁可见
   2. Vertex Buffer, Index Buffer, Uniform Buffer等等
4. Image：
   1. 在Vulkan中代表所有具有像素结构的数组，可以用于表示纹理，Render Target等等（类似一个MAT？）
   2. 和其他组件一样，Image也需要在创建的时候指定使用它的模式，例如Vulkan里有参数指定Image的内存Layout，可以是Linear，也可以是Tiled Linear便于纹理Filter。
   3. 纹理本身并不直接绑定给Pipeline。需要读取和使用Image则要依赖于ImageView。
5. Descriptor Sets
   1. 绑定资源
   2. **Buffer和Image可以动态的绑定给任意Pipeline。而具体绑定的规则就是由Descriptor指定。**
   3. Descriptor Set也需要在被创建的时候，就由App指定它的固定的Layout，**以减少渲染时候的计算量。**因为Descriptor Set是预先创建并且无法更改的，所以改变一个绑定的资源需要重新创建整个Descriptor Set，但改变一个资源的Offset可以非常快速的在绑定Descriptor Set的时候完成。
   4. Descriptor Set Layout可以指定绑定在指定Descriptor Set上的所有资源的种类和数量，以及在Shader中访问它们的索引。
   5. 看图,Descriptor Set需要在被创建的时候，就由App指定它的固定的Layout，**以减少渲染时候的计算量**
   6. ![](../pics/vkdescriptorlayout.png)
   7. 也就是说，setLayout描述有什么内容，set内部具体存这些内容
6. CommandBuffer：
   1. 在Vulkan里，没有任何API允许你直接的，立即的像GPU发出任何命令。
   2. 所有的命令，包括渲染的Draw Call，计算的调用，甚至内存的操作例如资源的拷贝，都需要通过App自己创建的Command Buffer。
   3. 相关函数一般都带有vkCmd。
   4. 每一个Command Buffer都需要显式的绑定它所需要的所有渲染状态，Shader，和Descriptor Set等等，没有任何渲染状态会在commandbuffer之间继承。（除非你设定特殊的Flag，让程序制定这些Command只会被调用一次（例如某些资源的初始化），亦或者应该被缓存从而重复调用多次（例如渲染循环中的某个Pass）。）这是为了让驱动能更加简易地优化command的调用。
7. Queue：
   1. 是Vulkan中唯一给GPU递交任务的渠道
   2. Queue的API极其简单，你向它递交任务（Command Buffer），然后如果有需要的话，你可以等待当前Queue中的任务完成。
   3. 基于Command Buffer和Queue递交任务的Vulkan非常易于编写多线程程序。
8. Pool：
   1. Command Pool：Command Buffer Pool是Command Buffer的父亲组件，负责分配Command Buffer。Command Buffer相关的操作会对其对应的Command Buffer Pool里造成一定的工作，例如内存分配和释放等等。因为多个线程会并行的进行Command Buffer相关的操作，这个时候如果所有的Command Buffer都来自同一个Command Buffer Pool的话，这时Command Buffer Pool内的操作一定要在线程间被同步。所以这里建议每个线程都有自己的Command Buffer Pool，这样每个线程才可以任意的做任何Command Buffer相关的操作。

   2. DescriptorSet Pool：所有Descriptor Set都由Descriptor Pool分配，Descriptor Set操作会导致对应的Descriptor Pool工作而且需要线程间同步，并且Descriptor Pool也支持非常高效的将所有由当前Pool分配的Descriptor Set一次性清零。所以程序应该为每个线程分配一个Descriptor Pool，可以根据Descriptor Set的更新频率，创建不同的Descriptor Pool，例如每帧、每场景等等。

### Vulkan编程基本流程

```cpp
//第一步：先创建一个Vulkan实例。每个实例之间完全孤立
VkInstance inst;
vkCreateInstance(&instanceCreateInfo, NULL, &inst);

//然后，向GPU获取信息，建立和硬件的联系
VkPhysicalDevice phys[4]; uint32_t physCount = 4;
vkEnumeratePhysicalDevice(inst, &physCount, phys);
VkDeviceCreateInfo deviceCreateInfo = {
   "blablabla";
}

//从物理硬件获取完信息之后，绑定到逻辑硬件上：
VkDevice device;
vkCreateDevice(phys[0], &deviceCreateInfo, NULL, &device);

// fetch vkCreateWin32SurfaceKHR extension function pointer via vkGetInstanceProcAddr
//与其他图形API一样，Vulkan将窗口系统方面与核心图形API分离开来。
//在Vulkan中，窗口系统的详细信息通过WSI（窗口系统集成）扩展公开。
//下面和Vulkan的显示界面相关，用来显示结果，Win32平台（Vulkan支持很多平台）
VkWin32SurfaceCreateInfoKHR surfaceCreateInfo = {
      // HINSTANCE, HWND, etc
   };
VkSurfaceKHR surface;
vkCreateWin32SurfaceKHR(inst, &surfaceCreateInfo, NULL, &surface);

//创建交换链。它是最终显示给用户的图像缓冲区列表。这是建立呈现所需的所有缓冲区所需的第一个步骤之一
//【参见附录1-交换链】
VkSwapchainCreateInfoKHR swapCreateInfo = {
    // surface goes in here
  };
VkSwapchainKHR swap;
vkCreateSwapchainKHR(device, &swapCreateInfo, NULL, &swap);





//有了一个逻辑Device之后，就可以开始创建其他东西了
//在Vulkan中，声明一个Buffer或者Image时，要声明一下他的用途
//同时，Image需要通过VkImageView来使用



```

【附录】

1. 交换链 SwapChain <br>
   ![](../pics/vkswapchain.png)






## VulkanRT部分

-----

8. 初始化和参数Setup
9. **加速结构**
1. 构建BLAS
2. 构建TLAS
3. 接口：
      1. 原代码封装的很好，还是需要自己拆出来一个最小系统才看得清
      2. 在Vulkan中，指令是通过类似于vkCmdxxx()的接口来写入到Command buffer，驱动会负责生成适配当前GPU的二进制指令内容。
