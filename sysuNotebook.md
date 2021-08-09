# (Note)Learning Semantic-Specific Graph Representation for Multi-Label Image Recognition
  
## 论文阅读记录

原文：
https://arxiv.org/pdf/1908.07325.pdf

目前还不清楚内涵的关键词：
1. multi-label recognition
2. semantic-aware
3. decoupling
4. 图表示学习

先搞清楚这些基本概念吧。
##### 多标签分类
找到一篇综述：
https://www.cnblogs.com/cxf-zzj/p/10049613.html

多标签的需求：以文章分类举例，一篇文章既可以属于“经济”也可以属于“文化”。
难点：
- 一个entity(不知道可不可以用这个单词形容)对应的类标数量相差巨大，可能对应1个也可能对应几百个
- 类标之间的相互依赖/耦合？
- 难以获取训练集，老生常谈了

解决方法：两大类(看起来没有啥特别针对的算法？)
1. 基于问题转化的方法
   1. 最简单的：单标签预测
   2. 要考虑关联性的话，用分类器链或者时序(前一个的out是后一个的in)
   3. 或者标签组合(但是根本没法用在标签维度高的情况下)
2. 基于算法适用的方法
   1. 主要是改造算法，似乎在深度学习领域会用到注意力机制，论文中的semantic-aware应该也是差不多的意思

##### semantic-aware
应该是用到了语义的这种“注意力”的机制，接着看下去应该就清楚了
##### decoupling
就是解耦吧
##### 图表示学习
之前一直没认真了解过图这方面的结构

参考：
https://zhuanlan.zhihu.com/p/85677181

图学习的作用：
节点分类，链接预测，社群检测，网络相似度
- Embedding: 一个样本的特征向量，不管它是否来自一个训练的网络，或是来自其他方法的构造，我们都可称之为Embedding。因为我们的最终目标是迭代出最优的特征向量，蕴含相似度信息，所以我们把这种特征向量也称之为Embedding.
难点在于抽取图结构数据的特征。
所以说所谓图学习的关键似乎在于如何高效地将输入数据(即图结构，nodes)映射到特征向量，并在这个过程中保留尽可能丰富的拓扑信息(所以叫"表示学习"?)
- 图表示学习的目标就是用n个向量表示图上的n个结点，这样就可以将一个难以表达的拓扑结构转化为可以丢进炼丹炉的 vector！
1. 图结构表示学习
   - Node2vec：单个节点到邻居节点跳跃的概率是不同的(有点像强化学习的马尔科夫链？)
   BFS: 宽度优先；DFS: 深度优先

2. 图特征表示学习
   - GCN：“结点i在下一层的表示是由其所有邻居节点j所构成的集合Neighbor(i)来决定的”，邻居结点的聚合。
   - GraphSAGE：只选择S个邻居节点进行聚合，有点像dropout，增加可计算性
   - GAT：加了注意力机制。(ps:论文公式中的||是concat操作)

论文中文译文参考：https://blog.csdn.net/weixin_43436958/article/details/111564594
#### Abstract： 
[Q1]
语义解耦模块和语义交互模块？一个实体所对应的标签之间是可能具有潜在联系的。挖掘标签的这种语义信息可能会对提高分类精度有益，但是听起来有点玄乎

[A1]
**看了后面的论文，大概懂训练的流程了。实际上所谓的语义解耦就是将类别词用Glove模型扩充到蕴含更多上下文信息的词向量，再用这个低秩双线性池化来融合图像特征和词向量最后加权，类似于按照不同权重同时学习多个类别标签**
#### Introduction：
**我们首先设计一个语义解耦模块，该模块利用类别的语义特征来指导学习与类别相关的图像特征，这些图像特征更加关注相应的语义区域。然后，我们构建一个基于统计标签共现的图来关联这些特征，并通过图传播机制来探索它们之间的相互作用**

#### Related Works：
提炼一下：最近多标签分类的进展多在于引入了对象定位，产生候选区域proposal(类比Faster-RCNN)，使得它可以注意到图像的各个子区域，分别进行预测，但是候选区域的产生和筛选难以被端到端训练

[Q1]
感觉和Faster-RCNN这些目标检测极其相似。。。那这么说，多标签图像分类完全没必要啊，因为它完全可以用目标检测模型来做不是吗???那为啥要搞得这么麻烦.....

[A1]**懂了，还是有区别的。图像分类毕竟是分类，数据集中标签只需要给出图像对应的类别就行，而不需要像目标检测一样给出BBox标定。目标检测确实是多标签图像分类的上位，但是他要的训练集成本也高得多，因此，设计一个只需要常规的图像分类数据集却能够接近目标检测那样多类检测的效果的模型，还是很有意义的**

也有用注意力机制来定位区域的，但是定位不精确.

或者也可以利用标签的依赖关系来建模(应该和作者的方法属于同一类的)，LSTM和RNN这种具有序列关系预测功能的网络就可以办到，但是并不能完全利用该特性(可能是因为他们只能按照时序?)

#### SSGRL Framework:
1. Overview

   关键：1、对于每个类别，语义解耦模块结合类别语义来指导学习特定于语义的表示
   2、语义交互模块使用基于统计标签共现构建的图来关联这些表示。

2. Semantic Decoupling:

   [Q1]
   这一部分的：
   >For each category c, the framework extracts a ds-dimensional semantic-embedding vector using the pre-trained GloVe model

   这个d维语义embedding vector是怎么来的？不能理解这里的fg是指代什么。wc是所谓“c类别对应的semantic word”，这里也没搞清楚

   [A1]
   **稍微懂一点了。这里是单独处理类别对应的词，用Glove模型训练出来一个对应的语义向量，赋予其语义信息。比如说输入car，返回它的向量，而这个向量又和track,bus,train等类似。等于说赋予标签以语义信息。**

   概括：这一步提取了1、图像的特征；2、该图像对应类别的词嵌入向量  然后用低秩双线性池化将他们融合，并通过一个全连接网络计算他们的注意系数。最后对这些注意系数进行归一化，并与上面的融合后的特征求加权平均。

   [Q2]
   低秩双线性池化?

   [A2]
   https://zhuanlan.zhihu.com/p/62532887

   是一种特征融合的手段
   原始的Bilinear Pooling存在融合后的特征维数过高的问题，融合后的特征维数=特征x和特征y的维数之积。
   改进：LRBP：Low-rank Bilinear Pooling
   怎么说，有点像是求两个特征的gram matrix, which is used in neural style transfer.


3. Semantic Interaction

   引入图G，其中节点指的是类别，边指的是对应类别之间的共现概率。
   每个节点可以聚集来自其他节点的消息，并同时通过图传递其信息，从而能够在对应于所有类别的所有特征向量之间进行交互,该过程重复T次，生成最终的隐藏状态。这样就可以学习一个各类别之间相关信息的隐藏表示了
   论文在这里将图结构写作{V,A}，V表示不同的类别词{v0,v1,v2…,v(c-1)}，A表示在c类别存在的条件下，c’类别存在的概率，A表示为{a00,a01,a02…,a0(c-1),…a(c-1)(c-1)}。

   https://blog.csdn.net/qq_45603919/article/details/109126254

   👆这篇文章在图网络这部分讲的很详细，省下不少时间。

   隐藏状态：将某个结点周围的信息融合成一个向量表示，因此可以用上一步生成的特征向量来初始化这个隐藏状态。
   特征聚合向量：某个类别对应的隐藏状态和两个共现概率的加权和。





4. Network Structure
   fcnn是ResNet-101,改了最后一个平均池化层

5. Optimization

## 代码复现记录

#### 15：30，8/1
开始干活

__目标是：搭建论文中的ResNet-101 多标签分类baseline，并在COCO数据集上训练调参，使其能达到约80.3的mAP。__

网络结构的描述：
> Specifically, we simply replace the last fully connected layer of the ResNet-101 with a 2,048-to-C fully connected layer and use C sigmoid functions to predict the probability of each category

去掉最后一个全连接层，换成一个2048-to-C的全连接层，然后使用C sigmoid函数去预测每一类的概率。

训练和测试设定：

损失函数用交叉熵

作者使用的imagenet预训练的ResNet-101模型

Adam优化器，batchsize = 4，动量为0.99和0.9

初始学习率：10^-5,误差趋于平稳时，再除以10.

input:640x640， {640, 576, 512, 384, 320}随机裁切patches，最后这些patches再resize到576x576.
对于testing，直接先resize to 640x640再中心裁切576x576

metrics：mean average precision。

每张图取得分最高的三个标签，并和实际值比对。

对于COCO数据集，再训练集上训练，在验证集上测试。

#### 16：38, 8/1

下载coco-2014数据集了，用迅雷在这个镜像站下载，听说比官网快很多

https://pjreddie.com/projects/coco-mirror/

但是有一个疑惑，coco数据集是目标检测的，要怎么改成multi-labels的

作者好像把这一步的处理直接写道datasets/cocodataset.py里了
```python
class CoCoDataset(data.Dataset):
    def __init__(self, image_dir, anno_path, input_transform=None, labels_path=None):

        ...
      
      self.labels = []
      if self.labels_path:
	      self.labels = np.load(self.labels_path).astype(np.float64)
         self.labels = (self.labels > 0).astype(np.float64)
 	   else:
         l = len(self.coco)
         for i in range(l):
            item = self.coco[i]
            print(i)
            categories = self.getCategoryList(item[1])
            label = self.getLabelVector(categories)               
            self.labels.append(label)


```
要不然先用一个阉割版的coco跑起来先吧。。。整个19G肯定是跑不起来了


#### 17：22, 8/1

谷歌了一下multi-label classification相关，发现腾讯在18年的时候也搞了个多标签检测的baseline。。。

pytorch上的一个讨论，看起来不错噢：

https://discuss.pytorch.org/t/transfer-learning-for-multi-label-classification-from-a-single-label-model/16755

感觉难点会出现在最后mAP的计算规则部分？虽然说可以看一下论文代码是怎么算的

#### 23：10，8/4
我先试着把论文代码的数据集读取和处理的代码框架移植过来

目前已有的环境：
| Env | Libs |
|-----|------|
|base|tf-gpu-1.13|
|TEST|torch-1.1.0|
|grad_cam|torch-1.5.0+cu101|
|tf2|tf-2.0.0|
|workspace|N/A|
|workspace37|tf-gpu-2.4.1|

经过测试，grad_cam环境是比较适合的

#### 0：22，8/4

刚刚解决了调用论文代码里cocodataset.py时出现的一个小bug。
本来是想直接调用cocodataset里的CoCoDataset类，但是首先发现论文代码里并没有提供缺少的category.json文件，coco数据集里也没有。猜想应该是类别名到id的一个映射，于是用了官方给的一个小脚本打印出了category和对应的id：

https://tech.amikelive.com/node-718/what-object-categories-labels-are-in-coco-dataset/

这样就有了category的json文件了。但是后来发现还是报错：
```python
   label = self.getLabelVector(categories)

  File "L:\sysuTask\ResNet-101-BM\cocodataset.py", line 56, in getLabelVector
    print("debug:",self.category_map[str(c)])

TypeError: list indices must be integers or slices, not str
```
然而print出来的c是整数。。。大胆猜测，论文作者的数据集的c应该是类别名，因此需要一个category.json映射到id，然而我的数据集的c已经是id了，那就不需多此一举，
```python
        for c in categories:
            index = c-1
            label[index] = 1.0 / label_num
        return label
```
改成这样就ok了，睡觉。

#### 16:05, 8/4
打算先用40504个样本，先生成list之后保存，以备下次再用。

```python
cuda:0 -> GeForce GTX 1050 is available.
loading annotations into memory...
Done (t=7.29s)
creating index...
index created!
  2%|▏         | 890/40504 [02:59<1:16:36,  8.62it/s]
```
python list的保存和读取：

https://blog.csdn.net/weixin_42769131/article/details/104242416?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.control&spm=1001.2101.3001.4242

数据流：

``class CoCoDataset(in cocodataset.py) -> train_set(in transforms.py) -> train_loader(in transforms.py) -> get_train_test_set(in main.py)``

#### 9：10，8/5
*args 和 **kwargs的用法

https://www.jianshu.com/p/0ed914608a2c

看了一下腾讯AIlab的resnet，最后一层：
```python
self.logit = self._fully_connected(x, out_dim=self.num_classes)
```
那么我就直接把最后一个fc改成
``self.fc = nn.Linear(2048, num_classes)``就好了

BCEWithLogitsLoss是用来计算多标签分类的误差的：

https://blog.csdn.net/qq_22210253/article/details/85222093

打算使用预训练+coco上finetune，pytorch加载与训练模型：
```python
state_dict = torch.load('trained.pth')
model.load_state_dict(state_dict)
```
这样可能会报错，因为总会有几个key对应不上，要用strich=False比较好：
```python
model.load_state_dict(state_dict, strict=False)
```
以上参考：
https://blog.csdn.net/t20134297/article/details/110533007

#### 8：37 8/6
昨晚挂了一晚竟然没成功保存下来，气死
今天又要花一个早上生成list了
``3%|▎         | 2881/82783 [02:40<7:58:02,  2.79it/s]``

半小时白给。。。
```python
OSError: image file is truncated (9 bytes not processed)
 29%|██▉       | 23807/82783 [37:31<2:02:18,  8.04it/s]
```
发现问题了，第23086张照片有问题。
要加一句``Image.LOAD_TRUNCATED_IMAGES = True``
https://blog.csdn.net/hzs3237259/article/details/108188323

但是改完coco.py中的代码后又报错了，报错如下
```python
TypeError: super(type, obj): obj must be an instance or subtype of type
```
猜测是coco.py作为一个被调用脚本在运行一次后如果修改它的内容，再调用时内核发现两者不一致而报的错，因此每次改完coco.py之后都需要restart kernel(好麻烦)

加了``Image.LOAD_TRUNCATED_IMAGES = True``还是不行啊。。。
啊这。。。。看了下，我本来以为coco.py只调用了Image所以就擅作主张把``ImageFile.LOAD_TRUNCATED_IMAGES = True``改成了``Image.LOAD_TRUNCATED_IMAGES = True``
但是实际上这样改没有用(也不知道为啥没报错)，如果没用到ImageFile就需要import imagefile，所以正确的做法应该是在coco.py中完整地加入下面两行代码：
```python
from PIL import ImageFile
ImageFile.LOAD_TRUNCATED_IMAGES = True
```
浪费半个小时。。。不过也没办法，这种错误碰到下次就知道了

#### 16：37 8/6

出了问题
```python
  File "D:\Anaconda\envs\grad_cam\lib\multiprocessing\reduction.py", line 60, in dump
    ForkingPickler(file, protocol).dump(obj)

BrokenPipeError: [Errno 32] Broken pipe
```
好像是因为多线程的原因?
看了一下源码，找到了。跟踪报错到 dataloader .py ,有：
```python
    def __iter__(self):
        if self.num_workers == 0:
            return _SingleProcessDataLoaderIter(self)
        else:
            return _MultiProcessingDataLoaderIter(self)
```
所以应该要把worker数设置成0

```python
  File "L:\sysuTask\ResNet-101-BM\resnet.py", line 157, in forward
    x = self.fc(x)
  File "D:\Anaconda\envs\grad_cam\lib\site-packages\torch\nn\modules\module.py", line 550, in __call__
    result = self.forward(*input, **kwargs)
  File "D:\Anaconda\envs\grad_cam\lib\site-packages\torch\nn\modules\linear.py", line 87, in forward
    return F.linear(input, self.weight, self.bias)
  File "D:\Anaconda\envs\grad_cam\lib\site-packages\torch\nn\functional.py", line 1612, in linear
    output = input.matmul(weight.t())
RuntimeError: size mismatch, m1: [73728 x 9], m2: [2048 x 90]
```
这个报错应该和我改resnet里的最后一层fc有关。我设定的fc是2048x90(class_num)，但是他这个mismatch到底是啥意思，
论文里面不也是说2048 to c。。。先把2048改成73728看看会不会报错，或者我在想要不要73728-2048-c这样加两层,
这样隐含层应该就是2048个节点了吧
```python
RuntimeError: size mismatch, m1: [73728 x 9], m2: [73728 x 90] 
```
但是这样还是不行，难道要完全一样吗。。。
懂了。。。pytorch的规则是[a,b],[b,c]这样，但是上一层怎么直接就到9了啊。。。

记录一下
```python
        x = self.layer1(x)
        x = self.layer2(x)
        x = self.layer3(x)
        x = self.layer4(x) [4, 2048, 18, 18]
        x = self.avgpool1(x) [4, 2048, 9, 9]
        x = x.view(x.size(0), -1) [4, 591872]
        x = self.fc(x) 
        
        				这种情况下报错：[147456 x 18] mismatch [2048 x 90]
        				对比一下应该是 4x2048x18 = 147456, 所以要搞一个类似于
        				reshape的东西，给他定型一下
        				顺便也解释了为什么avgpool会出现73728*9：
        				4 x 2048 x 18 x 18 -> 
        				147456 x 18 -> 
        				2654208 ->
        				avgpool的stride=2,所以要除以4,就有
        				2654208/4=663552 -> 73728 x 9 
```     			
最后还是直接手动设计后面的几层，因为一层的avgpooling出来的特征感觉太多了，怕直接全连接电脑吃不消，于是用了两层的pooling，然后直接(如果后面效果不好这是一个可以考虑改进的地方)：
```python
        定义：
        self.avgpool1 = nn.AvgPool2d(3, stride=2)
        self.avgpool2 = nn.AvgPool2d(2, stride=1)
        #self.fc = nn.Linear(8192, num_classes)
        #self.fc = nn.Linear(512 * block.expansion, num_classes)
        self.fc = nn.Linear(100352, num_classes) #[100532 = 2048*7*7]

        forward：
        x = self.layer4(x)
        x = self.avgpool1(x)
        x = self.avgpool2(x) #[4, 2048, 7, 7]
        #print(x.shape)
        x = x.view(x.size(0), -1)
        #print(x.shape)
        x = self.fc(x)
        #print(x.shape)
        #assert False
```   

跑起来了！！！
```python
Epoch: [0][0/20696]     Time 67.750 (67.750)    Loss 0.6822 (0.6822)
Epoch: [0][5/20696]     Time 1.461 (12.713)     Loss 0.6441 (0.6754)
Epoch: [0][10/20696]    Time 1.113 (7.569)      Loss 0.6201 (0.6552)
Epoch: [0][15/20696]    Time 1.153 (5.588)      Loss 0.5714 (0.6338)
Epoch: [0][20/20696]    Time 1.378 (4.573)      Loss 0.5413 (0.6116)
```
现在23：39，刚刚ctrl+c复制一下log结果keyboard interrupt了。。。反正今晚也跑不完，至少是能跑起来的。明天跑完看一下mAP吧(总感觉没这么简单)


#### 9:00 8/9
移植了一个jupyternotebook脚本，方便colab上跑：
https://github.com/satoshiSchubert/WorkSpace/blob/main/sysuTask.ipynb

很烦，colab上gpu有使用限额，tpu又要更改代码
现在在参照官方的pytorch tpu colab例程修改代码：
https://colab.research.google.com/github/pytorch/xla/blob/master/contrib/colab/resnet18-training.ipynb#scrollTo=8vMl96KLoCq8

还是失败了，报了一个很底层的数组越界错误，很难debug
现在创建了一个新的谷歌账号来蹭它的gpu

Colab上蹭的TeslaT4，一个iter按0.25s算，预计1.5h可以跑完

#### 13：37 8/9
一个半小时好不容易训练完一个epoch，结果validate()里面的tensor.item()忘记改，白搞

#### 18：22 8/9
目前是跑完两个Epoch了，我看loss值基本上下降得比较慢了(也有可能跑满200个Epoch还能有很可观的提升，但是还是得看mAP的值才能证明)
很奇怪的是，不知道是我代码没改好还是其他原因，colab没法把mAP值打印出来：
```python
Test: [10125/10126]	Time 0.215 (0.213)	Loss 0.0822 (0.1077)
(40504, 180)
 * mAP nan
```
不过万幸的是两个Epoch跑出来的checkpoint和score都保存下来了，并且score里存储了预测值和标签，应该是可以拿来直接计算的，但是就是得额外再写个脚本了。

#### 第一次训练结论(Epoch = 2)：

用excel看了一下生成的结果score文件，网络着重在分类person类了，其他类的输出的几乎全都是负值，准确度差很多。。猜测应该是person出现的频率最高且大大高于其他类，所以网络给person正值时可以使其loss较小。看来很可能是因为训练的Epoch不够，毕竟才2轮，虽然一轮就要一个半小时。多训练几轮相信会改善。

计算mAP的时候报错：
```python
RuntimeWarning: invalid value encountered in true_divide
```
这个问题应该是在0/0的时候造成的
通过在分子分母同时加上1e-10,完美解决。

但是第一轮算出来的mAP只有0.1016077459353008。。。。。。太低了吧





