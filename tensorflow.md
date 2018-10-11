# tensorflow知识
搭建双层LSTM
```
    lstm_qx = tf.contrib.rnn.BasicLSTMCell(n_hidden)
    lstm_hx = tf.contrib.rnn.BasicLSTMCell(n_hidden)
    ((outputs_fw, outputs_bw), (outputs_state_fw, outputs_state_bw)) = tf.nn.bidirectional_dynamic_rnn(lstm_qx, lstm_hx, inputs, dtype = tf.float32)

    final_state_h = tf.concat((outputs_state_fw.h, outputs_state_bw.h), 1)
```
```
tf.nn.bidirectional_dynamic_rnn()
 return：元组（（前传隐藏层输出，后传隐藏层输出），（（前传最后隐藏输出，记忆单元输出），（后传最后隐藏层输出，记忆单元输出））
 调用方式元组的调用方式
```