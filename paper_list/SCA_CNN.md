# SCA-CNN Spatial and Channel-wise Attention in Convolutional Networks for Image Captioning
## 原理
在image capitoning任务中，SCA_CNN在多层feature map中动态调节句子的生成上下文的生成。

作者认为，滤波器本身就是模型检测器，低层用来检测边缘和角，高层用来检测实体部分和目标，通过层的堆叠，CNN通过视觉抽象层能够提取图片特征，因此CNN图片特征本来就是spatial，channel-wise，和multi-layer。但是现存的很多注意力模型只关注空间特征。

多通道的feature map 本质上是相应的滤波器的检测响应图，多通道的attention被看做是在句子上下文的信息条件下选择语义属性的过程。

例如，当我们想预测蛋糕时，我们的多通道注意力模型通过根据像蛋糕，火，光和candle-like等类型的滤波器会施加更多的权重在多通道特征图

但是文章中没写多层怎么加的

## 以前的缺点
之前的注意力模型，只关注于最后一层的输出，VGG-16 14*14*512，最后一层所对应的感受野比较大，而且不同的感受野之间的不同也会受到限制

## 整体结构

SCA-CNN通过在多层的channel-wise注意力和spatial注意力使原始的CNN多层feature map适应句子上下文
在生成feature的调节策略是，不是将所有的feature map乘以权重加起来，而是使用点积相乘，


channel-wise attention 机制CNN feature map的channel代表的是相应的卷积滤波器的响应激活。因此，channel-wise方法可以被看做选择语义属性的过程

## 问题：
为什么要使用点积，在生成对视觉的特征图的时候