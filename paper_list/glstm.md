2015 guiding long-short term memory for image caption generation
1.核心思想：

对LSTM进行了扩展，加入图片的语义信息引导LSTM训练。通过对与图片内容有语义关系的词增加一个积极的权重来保持“on track”。

这个额外的语义信息可以有很多方式只要简历图片和其描述的语义连接。例如一个语义编码，一个分类或者说检索结果。这篇论文中不仅使用基于CCA的多模型语义嵌入，而且使用基于语义嵌入的交叉模型检索结果。

We notice that sometimes the generated sentence seems to “drift away” or “lose track” of the original image content, generating a description that is common in the dataset, yet only weakly coupled to the input image. We hypothesize this is because the decoding step needs to find a balance between two, sometimes contradicting, forces: on the one hand, the sentence to be generated needs to describe the image content; on the other hand, the generated sentence needs to fit the language model, with more likely word combinations to be preferred. The system then may “lose track” of the original image content if the latter force starts to dominate





2.相对于传统的提升：

作者认为语义信息表示文字描述和图片的相关性。
最近提出的注意力模型在生成单词的时候都是注意局部的信息，但是gLStm关注的是全局线索。


3.semantic information

（1）交叉模型检索任务和简单的使用检索句子作为语义信息
用一张图片检索出来的文本信息作为辅助信息，并将他们加入到自然语言模型，虽然这些句子对于图片来说并不准确，但是，他们提供了图片的丰富的语义信息，使用CCA模型。

（2）语义信息也能用视觉和语义代表相同过的语义嵌入空间编码说明，使用CCA产生的embedding space

（3）图片信息
CCA产生的embedding 空间，可以通过gLSTM模型自己学习映射矩阵，将图片信息映射到空间的映射矩阵，因为矩阵相当于是一个线性变换

4.长度归一化的定向搜索
考虑图片和文字的指数型搜索空间，详尽的搜索是不可能高的，所以采用了探索式搜索策略。采用定向搜索策略。

5.为什么要这么设计？
作者认为


