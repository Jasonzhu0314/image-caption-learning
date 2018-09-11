# Show And Tell Attention
## 一、简介
这篇论文介绍了两个基于注意力机制的图片标题生成模型
1.“soft”确定注意力机制，使用标准的BP进行训练
2.“hard”随机注意力机制，通过最大化近似差分低边缘或者等于reinforce
论文中描述，我们能够通过可视化“where”和“what”注意点来获得框架的内在和解释结果。
本论文与其他论文的不同点：
我们的模型并不准确学习目标检测，而是通过抓取获得最新的对齐，我们的模型趋向于学习抽象概念。
## 二、 模型
### 2.1 模型细节
#### 2.1.1 整体网络结构：
![](https://github.com/Jasonzhu0314/image-caption-learning/blob/master/pics/show_attention/network.jpg)
#### 2.1.2 Encoder：
获取图像的底层特征，论文中选取了VGGnet提取特征，选取了全连接层之前的feature map，大小为14x14x512，图像作为Decoder的输入为：

ai代表图片的中的一部分，原图片经过卷积和池化之后，后面的层代表的是原图片中较大部分的区域
#### 2.1.3 Decoder：
RNN内部使用的LSTM，LSTM具体细节不在说明

attention α 的生成，跟图片在i时刻对应的图片的位置有关，还跟Decoder过程中的时序有关，用理论解释说明就是，网络下一时刻“where”的地方跟之前已经生成的单词有关
tensorflow代码：
```
def get_alpha(self, prev_h, pctx):
  # projected state
    pstate = slim.fully_connected(prev_h, self.att_hid_size, activation_fn = None, scope = 'h_att') # (batch * seq_per_img) * att_hid_size
    pctx_ = pctx + tf.expand_dims(pstate, 1) #(batch * seq_per_img) * 196 * att_hid_size
    pctx_ = tf.nn.tanh(pctx_) # (batch * seq_per_img) * 196 * att_hid_size
    alpha = slim.fully_connected(pctx_, 1, activation_fn = None, scope = 'alpha') # (batch * seq_per_img) * 196 * 1
    alpha = tf.squeeze(alpha, [2]) # (batch * seq_per_img) * 196
    alpha = tf.nn.softmax(alpha)
  return alpha
  # prev_h代表上一时刻RNN的隐藏层输出，pctx代表图片的各个part
```

（1）确定的“soft” 注意力机制
在每次图片作为输入的时候，会把每个a中所有L维度进行一个加权α，然后计算的结果作为LSTM的输入

（2）随机（stochastic）的“hard”注意力机制
在每次图片L维度作为输入的时候，会把L个维度中的一个作为输入，s作为一个one-hot向量，s在输入的时候要进行随机抽样，s被多项分布（multinuolli distribution）α参数化

论文中定义了一个新的loss function

在更新权重的时候做了一个平滑处理，在训练的过程中，一句话中对每个单词的计算的loss，并计算所有单词的总loss，并进行平均：


