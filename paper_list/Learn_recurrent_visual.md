# Learning a Recurrent Visual Representation for Image Caption Generation
## 1. 核心思想：
提出了一个双向循环神经网络来映射图片和基于句子的表示，能够从图片中生成新的描述和从文字中生成新的视觉代表。
精神图片的创建在人的句子理解中占有很重要的部分，在理解和生成图片描述的时候，视觉存储信息究竟在计算机视觉算法中起到什么作用？
对于视觉记忆的两个关键问题就是（1）对于语言建立语言模型，（2）理解图片的语义内容，原始的一些做法认为将图片和文本关联起来映射到同一空间，但是给出图像和文本之间的高非线性的映射，在基于浅层表示的情况下很难找到一个一般的距离评价。
## 2. 解决的问题：
不像以前的一些论文是将句子和图片映射到共同的空间编码，不仅通过以前的字预测下一个字，而且通过预测视觉信息
## 3. 目标：
（1）我们通过给一些视觉的特征，希望能生成句子，特别的来说，给一些以前生成的词和视觉特征来生成下一个词。
（2）我们也提高计算视觉特征V的似然性的性能，通过给一些用于生成视觉表示或者用于图片检索的词。