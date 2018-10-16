# 在keras上的多GPU训练的理解
## 1. 过程
（1）这个函数将模型从CPU复制到所有gpu，从而获得单台机器、多gpu数据并行性
（2）当训练我们的网络时，will be batched to each of the GPUs。（这里指的是同一个batch都在每个GPU上训练还是说不同的batch在每个GPU上训练？），CPU将从每个GPU获得渐变，然后执行渐变更新步骤

## 2. 显卡显存中存放的数据
| 数据 | VGG |
|--------|--------|
|   图片  |(RGB)224 x 224 x 3 x batch（16）约为 2.4MB  |
|模型参数|权重：138M x 32 bit/8 = 552MB x batch(16) 约为8.8GB |
|卷积计算中间结果|24M x 32 bit/8 = 96MB x batch(16) 约为1.56GB |

## 3. tensorflow中的GPU机制
tensorflow框架在定义的时候默认会选择GPU去定义网络模型，如果想要在CPU上运行代码，需要将模型定义在CPU上。

tensorflow中的代码实现在CPU上运行,默认情况下，即使机器有很多个CPU，tensorflow也不会区分它们，所有CPU都使用/CPU:0作为名称
```
# 新建一个graph.
with tf.device('/cpu:0'):
  a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
  b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
c = tf.matmul(a, b)
# 新建session with log_device_placement并设置为True.
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
# 运行这个op.
print sess.run(c)
```
```
Device mapping:
/job:localhost/replica:0/task:0/gpu:0 -> device: 0, name: Tesla K40c, pci bus
id: 0000:05:00.0
b: /job:localhost/replica:0/task:0/cpu:0
a: /job:localhost/replica:0/task:0/cpu:0
MatMul: /job:localhost/replica:0/task:0/gpu:0
[[ 22.  28.]
 [ 49.  64.]]
```

## 4.keras保存多GPU训练模型
Modelcheckpoint callback 报错
Modelchecpoint callback函数无法调用也是因为保存时调用的是paralleled_model.save()导致的。
解决方法一：在modelcheckpoint里参数加上save_weights_only=True后，只会保存模型权重，但是保存的模型只能在同样数量的GPU上载入，而没办法再单GPU下载入。

解决方法二：自定义 Callback 函数，新写一个继承于keras中callback的新的类
```
original_model = ...
parallel_model = multi_gpu_model(original_model, gpus=n)

class MyCbk(keras.callbacks.Callback):

    def __init__(self, model):
         self.model_to_save = model

    def on_epoch_end(self, epoch, logs=None):
        self.model_to_save.save('model_at_epoch_%d.h5' % epoch)

cbk = MyCbk(original_model)
parallel_model.fit(..., callbacks=[cbk])
```
## 5.CPU和GPU之间的通信
CPU需要控制传输过程，不过现在为了节省CPU占用，都使用DMA传输，训练所需要的数据都需要从硬盘（Hard Disk Drive， HDD）中加载到系统内存（Random Access Memory，RAM）中。然后，网格和纹理等数据又被加载到显卡上的存储空间：显存（Video Random Access Memory，VRAM）中。这是因为显卡对于显存的访问速度更快。


