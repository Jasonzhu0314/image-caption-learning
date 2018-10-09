# SCA-CNN Spatial and Channel-wise Attention in Convolutional Networks for Image Captioning
## 原理
使用三种注意力模型，spatial attention：多层注意力模型





多通道的feature map 本质上是相应的滤波器的检测响应图，多通道的attention被看做是在句子上下文的信息中选择语义属性的过程。

例如，当我们想预测蛋糕时，我们的多通道注意力模型通过根据像蛋糕，火，光和candle-like等类型的滤波器会施加更多的权重在多通道特征图

## 以前的缺点
之前的注意力模型，只关注于最后一层的输出，VGG-16 14*14*512，最后一层所对应的感受野比较大，而且不同的感受野之间的不同也会受到限制

## 整体结构

SCA-CNN通过在多层的channel-wise注意力和spatial注意力使原始的CNN多层feature map适应句子上下文
在生成feature的调节策略是，不是将所有的feature map乘以权重加起来，而是使用点积相乘，


channel-wise attention 机制CNN feature map的channel代表的是相应的卷积滤波器的响应激活。因此，channel-wise方法可以被看做选择语义属性的过程