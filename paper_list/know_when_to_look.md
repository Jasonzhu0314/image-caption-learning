# know when to look：Adaptive Attention via A visual sentinel for Image captioning
## 1.简介
论文认为在生成预测单词的过程中，有些单词可能不需要加入图片信息去预测，比如说“the”和“of”，还有有些词预测的时候可能是依赖固定的语言模型，sign一般在“behind a red stop ”的后面，所以作者提出了一种基于“视觉哨兵”的新的自适应的注意力模型，在预测单词的时候，由模型决定是否加入关注图片的信息。
实际上，训练过程中，非语言信号产生的梯度，在预测单侧生成过程中，将会误导或者减少视觉信号的整体有效性。

## 2. 网络结构
![](https://github.com/Jasonzhu0314/image-caption-learning/tree/master/pics/know_when/know_when_to_look.png)

## 3. 什么是视觉哨兵（核心思想）
LSTM记忆单元存储长和短的视觉和语言信息。我们从记忆单元中提取新的成分，当模型选择是否关注图片的时候可以依靠这个成分，这个成分就是视觉哨兵。设计一个门限用来控制memory单元，公式如下：
![](https://github.com/Jasonzhu0314/image-caption-learning/tree/master/pics/know_when/visual_sentinel.png)
Xt代表t时刻时，LSTM的输入，gt是应用在memory单元mt的控制门，·代表点积

在预测单词生成的时候，比如说模型“white”, “bird”, “red” and “stop”会学习更注意图片的信息，在生成“top”, “of” and “sign”时会更注意视觉哨兵的信息。

## 4. 空间注意力模型
原来：用上一时刻LSTM的隐藏层单元和imag特征来产生权重，并加权到V上，作为LSTM的输出。

现在：LSTM的输入是整张图片的feature和上一个的单词，将输出单元和memory单元分别拿出来，memory单元用于控制视觉哨兵，来控制在预测的时候是否关注视觉，
这个在预测的时候加入的图片的信息跟mRNN的后面阶段很像。
