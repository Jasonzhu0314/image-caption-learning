# image-caption
每日image caption论文的阅读笔记
## 论文分类：
### 研究方向
#### 1. 基于多模型嵌入学习方式

1.2014_Images with Multimodal Recurrent Neural Networks. Junhua Mao1;2 Wei Xu1 Yi Yang1 Jiang Wang1 Alan L. Yuille Baidu Research

2.I. Sutskever, J. Martens, and G. Hinton. Generating text with recurrent neural networks. In ICML, 2011.

#### 2. 基于语言模型
1.A. Gupta, Y. Verma, and C. Jawahar. Choosing linguistics over vision to describe images. In AAAI, 2012

2.M. Mitchell, X. Han, J. Dodge, A. Mensch, A. Goyal, A. Berg, K. Yamaguchi, T. Berg, K. Stratos, and H. Daum´e III. Midge: Generating image descriptions from computer vision detections. In EACL, 2012.
- - -
#### 已读论文

| 模型结构 | 主要研究 |
|--------|--------|
|2015_CVPR_NIC|   只在语言模型的第一阶段加入图片信息     |
|2015_ICCV_gLSTM|对NIC模型的改进，在语言模型循环阶段加入三种不同的信息比较结果 |
|2015_show and tell|在图像信息提取阶段采用了googlenet，优点在于它的训练的过程比较好|
|2016_show attention and tell|改变语言模型的输入，在生成单词的时候关注图像的不同区域|
|2018_know when to look| 在生成某些词的时候对于以前记忆的词的依赖高于对图片信息的依赖，学习一种β权重决定生成词的时候图片信息和以前存储的信息的占比|
|2015_ICLR_m-RNN|在做多模型嵌入的时候，将来源于三个不同方面相加而不是拼接，并加入两层word embedding|
|LRCN|在语言模型阶段使用了两层stack LSTM的语言模型,应用在视频动作识别，图片描述|
|learning recurrent visual represent|在基础结构不变的情况下，语言模型使用RNN，并在文本生成的时候对图片特征进行重构|


- - -



1.[2015_show attention and tell](https://github.com/Jasonzhu0314/image-caption-learning/blob/master/paper_list/show_and_tell_attention.md)

2.[2017_Konwing when to look：adaptive attention via A visual sentinel for image captioning](https://github.com/Jasonzhu0314/image-caption-learning/blob/master/paper_list/konw_when_to_look.md)

3.[2015_CVPR_Deep Visual-Sementic Alignment for Generating Image Descriptions](https://github.com/Jasonzhu0314/image-caption-learning/blob/master/paper_list/Deep_Visual_Semantic_Alignments.md)

4.