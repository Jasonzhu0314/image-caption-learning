# 在keras上的多GPU训练的理解
## 1. 过程
（1）这个函数将模型从CPU复制到所有gpu，从而获得单台机器、多gpu数据并行性
（2）当训练我们的网络时，will be batched to each of the GPUs。（这里指的是同一个batch都在每个GPU上训练还是说不同的batch在每个GPU上训练？），CPU将从每个GPU获得渐变，然后执行渐变更新步骤
