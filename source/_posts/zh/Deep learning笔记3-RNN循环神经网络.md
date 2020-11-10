
---
title: Deep learning笔记3-RNN循环神经网络
lang: zh
date: 2017-10-16 18:18:56
tags: Deep Learning
---

### 1. 循环神经网络（RNN）

原图和公式说明来自：[零基础入门深度学习(5) - 循环神经网络](https://zybuluo.com/hanbingtao/note/541458 "Title") 

-------------------------------------

<center>![RNN](/image/DL/3/1-1.jpg)</center> 

-------------------------------------

x是一个向量，表示输入层的值（这里面没有画出来表示神经元节点的圆圈）；

s是一个向量，表示隐藏层的值（这里隐藏层面画了一个节点，也可以想象这一层其实是多个节点，节点数与向量s的维度相同）；

U是输入层到隐藏层的权重矩阵；

o是一个向量，表示输出层的值；

V是隐藏层到输出层的权重矩阵。

W权重矩阵是隐藏层上一次的值作为这一次的输入的权重矩阵。

#### 1.1. 循环神经网络的计算

##### 1.1.1. 基本循环神经网络

\begin{align}
\mathrm{o}_t&=g(V\mathrm{s}_t)\qquad\qquad\quad(输出层)\\
\mathrm{s}_t&=f(U\mathrm{x}_t+W\mathrm{s}_{t-1})\qquad(隐藏层)\\
\end{align}

如果反复把隐藏层带入到输出层得到：

\begin{align}
\mathrm{o}_t&=g(V\mathrm{s}_t)\\
&=Vf(U\mathrm{x}_t+W\mathrm{s}_{t-1})\\
&=Vf(U\mathrm{x}_t+Wf(U\mathrm{x}_{t-1}+W\mathrm{s}_{t-2}))\\
&=Vf(U\mathrm{x}_t+Wf(U\mathrm{x}_{t-1}+Wf(U\mathrm{x}_{t-2}+W\mathrm{s}_{t-3})))\\
&=Vf(U\mathrm{x}_t+Wf(U\mathrm{x}_{t-1}+Wf(U\mathrm{x}_{t-2}+Wf(U\mathrm{x}_{t-3}+...))))
\end{align}

##### 1.1.2. 双向循环神经网络

-------------------------------------

<center>![RNN](/image/DL/3/1-2.jpg)</center> 

-------------------------------------
最终的输出值取决于和。其计算方法为：

\begin{align}
\mathrm{y}_2=g(VA_2+V'A_2')
\end{align}

其中:

\begin{align}
A_2&=f(WA_1+U\mathrm{x}_2)\\
A_2'&=f(W'A_3'+U'\mathrm{x}_2)\\
\end{align}

正向计算时，隐藏层的值St与St-1有关；反向计算时，隐藏层的值S't与S't+1有关；最终的输出取决于正向和反向计算的加和。双向循环神经网络的计算方法：

\begin{align}
\mathrm{o}_t&=g(V\mathrm{s}_t+V'\mathrm{s}_t')\\
\mathrm{s}_t&=f(U\mathrm{x}_t+W\mathrm{s}_{t-1})\\
\mathrm{s}_t'&=f(U'\mathrm{x}_t+W'\mathrm{s}_{t+1}')\\
\end{align}

正向计算和反向计算不共享权重: U和U'、W和W'、V和V'都是不同的权重矩阵。

##### 1.1.3. 深度循环神经网络

-------------------------------------

<center>![RNN](/image/DL/3/1-3.jpg)</center> 

-------------------------------------

堆叠两个以上的隐藏层，就得到了深度循环神经网络。其计算方法为：

\begin{align}
\mathrm{o}_t&=g(V^{(i)}\mathrm{s}_t^{(i)}+V'^{(i)}\mathrm{s}_t'^{(i)})\\
\mathrm{s}_t^{(i)}&=f(U^{(i)}\mathrm{s}_t^{(i-1)}+W^{(i)}\mathrm{s}_{t-1})\\
\mathrm{s}_t'^{(i)}&=f(U'^{(i)}\mathrm{s}_t'^{(i-1)}+W'^{(i)}\mathrm{s}_{t+1}')\\
...\\
\mathrm{s}_t^{(1)}&=f(U^{(1)}\mathrm{x}_t+W^{(1)}\mathrm{s}_{t-1})\\
\mathrm{s}_t'^{(1)}&=f(U'^{(1)}\mathrm{x}_t+W'^{(1)}\mathrm{s}_{t+1}')\\
\end{align}

#### 1.2. 循环神经网络的训练

- 循环神经网络的训练算法：BPTT  

先前向传播，再反向传播，利用链式求导计算损失函数对每个权重的偏导数（梯度），然后再根据梯度下降公式更新权重w。

- 前向计算

\begin{align}
\mathrm{s}_t=f(U\mathrm{x}_t+W\mathrm{s}_{t-1})
\end{align}

- 误差项的计算

BTPP算法将第l层t时刻的误差项值沿两个方向传播：

① 是沿空间传递到上一层网络，这部分只和权重矩阵U有关：

\begin{align}
(\delta_t^{l-1})^T=&(\delta_t^l)^TUdiag[f'^{l-1}(\mathrm{net}_t^{l-1})]
\end{align}

② 是沿时间线传递到初始时刻，这部分只和权重矩阵W有关：

\begin{align}
\delta_k^T=&\delta_t^T\prod_{i=k}^{t-1}Wdiag[f'(\mathrm{net}_{i})]
\end{align}

RNN在训练中很容易发生梯度爆炸和梯度消失（取决于大于1还是小于1），这导致训练时梯度不能在较长序列中一直传递下去，从而使RNN无法捕捉到长距离的影响。

### 2. 长短时记忆网络（LSTM）

原图和公式说明来自：- [零基础入门深度学习(6) - 长短时记忆网络](https://zybuluo.com/hanbingtao/note/581764 "Title") 

-------------------------------------

<center>![RNN](/image/DL/3/2-1.jpg)</center> 

-------------------------------------

长短时记忆网络相对于普通循环神经网络，增加了一个状态c来保存长期的状态。

-------------------------------------

<center>![RNN](/image/DL/3/2-2.jpg)</center> 

-------------------------------------

#### 2.1. 长短时记忆网络的计算

-------------------------------------

<center>![RNN](/image/DL/3/2-3.jpg)</center> 

-------------------------------------

\begin{align}
g(\mathbf{x})=\sigma(W\mathbf{x}+\mathbf{b})
\end{align}

● 遗忘门（forget gate）决定了上一时刻的单元状态有多少保留到当前时刻：

\begin{align}
\mathbf{f}_t=\sigma(W_f\cdot[\mathbf{h}_{t-1},\mathbf{x}_t]+\mathbf{b}_f)
\end{align}

● 输入门（input gate）决定了当前时刻网络的输入有多少保存到单元状态：

\begin{align}
\mathbf{i}_t=\sigma(W_i\cdot[\mathbf{h}_{t-1},\mathbf{x}_t]+\mathbf{b}_i)
\end{align}

● 输出门（output gate）来控制单元状态有多少输出到LSTM的当前输出值：

\begin{align}
\mathbf{\tilde{c}}_t=\tanh(W_c\cdot[\mathbf{h}_{t-1},\mathbf{x}_t]+\mathbf{b}_c)
\end{align}

推荐阅读 - [强的笔记-我对LSTM的理解](http://blog.qiangzhonghua.com/blog/tech/lstm#15-%E9%81%97%E7%95%99%E9%97%AE%E9%A2%98---%E5%8F%8D%E5%90%91%E4%BC%A0%E6%92%AD%E5%85%AC%E5%BC%8F "Title")

-------------------------------------

#### 2.2. 长短时记忆网络的训练

先前向传播，再反向传播，利用链式求导计算损失函数对每个权重的偏导数（梯度），然后再根据梯度下降公式更新权重w。

第l层t时刻的误差项值沿两个方向传播：

① 是沿空间传递到上一层网络，这部分只和权重矩阵U有关：

\begin{align}
\frac{\partial{E}}{\partial{\mathbf{net}_t^{l-1}}}&=(\delta_{f,t}^TW_{fx}+\delta_{i,t}^TW_{ix}+\delta_{\tilde{c},t}^TW_{cx}+\delta_{o,t}^TW_{ox})\circ f'(\mathbf{net}_t^{l-1})
\end{align}

② 是沿时间线传递到初始时刻，这部分只和权重矩阵W有关：

\begin{align}
\delta_k^T=\prod_{j=k}^{t-1}\delta_{o,j}^TW_{oh}
+\delta_{f,j}^TW_{fh}
+\delta_{i,j}^TW_{ih}
+\delta_{\tilde{c},j}^TW_{ch}
\end{align}

### 3. 基于TensorFlow的实现（RNN/LSTM with TF）

TensorFlow中的RNN的抽象基类“ [RNNCell](https://www.tensorflow.org/api_docs/python/tf/contrib/rnn/RNNCell "Title")”，是实现RNN的基本单元。

Github - [源码阅读](https://github.com/tensorflow/tensorflow/blob/r1.2/tensorflow/python/ops/rnn_cell_impl.py "Title")

● call方法

每个RNNCell都有一个call方法，使用方式是：(output, next_state) = call(input, state)。

每调用一次RNNCell的call方法，就相当于在时间上推进了一步。

假设有一个初始状态h0，还有输入x1，调用call(x1, h0)后就可以得到(output1, h1)：
  
-------------------------------------

<center>![RNN](/image/DL/3/3-1.jpg)</center> 

-------------------------------------

再调用一次call(x2, h1)就可以得到(output2, h2)：

-------------------------------------

<center>![RNN](/image/DL/3/3-2.jpg)</center> 

-------------------------------------

● 两个重要类变量

隐藏层的大小：state_size

输出层的大小：output_size

● 常用的三个子类

[BasicRNNCell](https://www.tensorflow.org/versions/master/api_docs/python/tf/contrib/rnn/BasicRNNCell "Title")

[BasicLSTMCell](https://www.tensorflow.org/versions/master/api_docs/python/tf/contrib/rnn/BasicLSTMCell "Title")

[MultiRNNCell](https://www.tensorflow.org/versions/master/api_docs/python/tf/contrib/rnn/MultiRNNCell "Title")

#### 3.1. 单层LSTM例

```python

import tensorflow as tf
import numpy as np
lstm_cell = tf.nn.rnn_cell.BasicLSTMCell(num_units=128)
inputs = tf.placeholder(np.float32, shape=(32, 100)) # 32 是 batch_size
h0 = lstm_cell.zero_state(32, np.float32) # 得到一个全0的初始状态
output, h1 = lstm_cell.call(inputs, h0)

# LSTM可以看做有两个隐状态h和c，对应的隐层就是一个Tuple元组
# 每个都是(batch_size, state_size)的形状
print(h1.h)  # shape=(32, 128)
print(h1.c)  # shape=(32, 128)

```
#### 3.2. 一次执行多步

TensorFlow提供了一个tf.nn.dynamic_rnn函数，使用该函数就相当于调用了n次call函数。

```python
# inputs: shape = (batch_size, time_steps, input_size) 
# cell: RNNCell
# initial_state: shape = (batch_size, cell.state_size)。初始状态。一般可以取0矩阵
outputs, state = tf.nn.dynamic_rnn(cell, inputs, initial_state=initial_state)

```

官方 [tf.nn.dynamic_rnn文档](https://www.tensorflow.org/api_docs/python/tf/nn/dynamic_rnn "Title")
 
#### 3.3. 堆叠RNNCell

即实现多层的RNN。将x输入第一层RNN的后得到隐层状态h，这个隐层状态就相当于第二层RNN的输入，第二层RNN的隐层状态又相当于第三层RNN的输入，以此类推。


```python
import tensorflow as tf
import numpy as np

# 每调用一次这个函数就返回一个BasicRNNCell
def get_a_cell():
    return tf.nn.rnn_cell.BasicRNNCell(num_units=128)
# 用tf.nn.rnn_cell MultiRNNCell创建3层RNN
cell = tf.nn.rnn_cell.MultiRNNCell([get_a_cell() for _ in range(3)]) # 3层RNN
# 得到的cell实际也是RNNCell的子类
# 它的state_size是(128, 128, 128)
# (128, 128, 128)并不是128x128x128的意思
# 而是表示共有3个隐层状态，每个隐层状态的大小为128
print(cell.state_size) # (128, 128, 128)

# 使用对应的call函数
inputs = tf.placeholder(np.float32, shape=(32, 100)) # 32 是 batch_size
h0 = cell.zero_state(32, np.float32) # 通过zero_state得到一个全0的初始状态
output, h1 = cell.call(inputs, h0)

print(h1) # tuple中含有3个32x128的向量

```
MultiRNNCell也是RNNCell的子类，因此也有call方法、state_size和output_size变量。同样可以通过tf.nn.dynamic_rnn来一次运行多步。

- Attention 版本问题

目前使用TensorFlow 1.2运行堆叠RNN的代码是：

```python
# 每调用一次这个函数就返回一个BasicRNNCell
def get_a_cell():
    return tf.nn.rnn_cell.BasicRNNCell(num_units=128)
# 用tf.nn.rnn_cell MultiRNNCell创建3层RNN
cell = tf.nn.rnn_cell.MultiRNNCell([get_a_cell() for _ in range(3)]) # 3层RNN

```
更早一些的版本中（比如TensorFlow 1.0.0），实现方式是：

```python
one_cell = tf.nn.rnn_cell.BasicRNNCell(num_units=128)
cell = tf.nn.rnn_cell.MultiRNNCell([one_cell] * 3) # 3层RNN

```
新版本按照这种的方式定义，就会引起报错。

阅读参考 - [RNN入门：多层LSTM网络（四）](http://www.jianshu.com/p/b3c7883e3ddf "Title") 

#### 3.4. TF循环神经网络的实现例

Make TV scripts using RNN(Recurrent Neural Networks) and LSTM(Long Short-Term Memory) network. Generate a new TV script with the model from TV scripts training data sets.

RNN(Recurrent Neural Networks)及びLSTM(Long Short-Term Memory)を使用して、訓練データを用いたモデルを作成して、新しいテレビスクリプトを生成する。

程序实例 - [Github Link](https://github.com/HJTSO/tv-script-generation/blob/master/dlnd_tv_script_generation.ipynb "Title") 

### 4. 词分析框架（Word Framework）

#### 4.1. word2vec

单词嵌入模型（简称为  word2vec 模型）。将word映射成连续（高维）向量，这样通过训练，就可以把对文本内容的处理简化为K维向量空间中向量运算，而向量空间上的相似度可以用来表示文本语义上的相似度。

一般来说， word2vec输出的词向量可以被用来做NLP相关的工作，比如聚类、找同义词、词性分析等等。

有两种实现的架构，CBOW(Continuous Bag-Of-Words)模型以及Skip-gram模型：

-------------------------------------

<center>![RNN](/image/DL/3/4-1.png)</center> 

-------------------------------------

阅读链接 - [word2vec入门基础](http://www.cnblogs.com/tina-smile/p/5176441.html "Title")


#### 4.2. seq2seq

序列到序列模型（简称为 seq2seq 模型）。seq2seq模型就像一个翻译模型,输入是一个序列(比如一个英文句子),输出也是一个序列(比如该英文句子所对应的法文翻译)。

-------------------------------------

<center>![RNN](/image/DL/3/4-2.png)</center> 

-------------------------------------

● 编码器：这是一个[tf.nn.dynamic_rnn](https://www.tensorflow.org/api_docs/python/tf/nn/dynamic_rnn "Title")函数。

● 解码器：这是一个[tf.contrib.seq2seq.dynamic_rnn_decoder](https://www.tensorflow.org/versions/r1.0/api_docs/python/tf/contrib/seq2seq/dynamic_rnn_decoder "Title")函数。

-------------------------------------

<center>![RNN](/image/DL/3/4-3.jpg)</center> 

-------------------------------------

阅读链接 - [Seq2Seq的DIY简介](http://www.jianshu.com/p/124b777e0c55 "Title")

阅读链接 - [聊天机器人深度学习](http://www.wildml.com/2016/04/deep-learning-for-chatbots-part-1-introduction/ "Title")

阅读链接 - [tensorflow-seq2seq-tutorials ](https://github.com/ematvey/tensorflow-seq2seq-tutorials "Title")

-------------------------------------

● word2vec输出的是一个词向量，seq2seq输出的是一个词序列。

推荐阅读 - [Deep Learning for Chatbots, Part 1](http://www.wildml.com/2016/04/deep-learning-for-chatbots-part-1-introduction/ "Title")

推荐阅读 - [《安娜卡列尼娜》文本生成](https://zhuanlan.zhihu.com/p/27087310 "Title")

推荐阅读 - [LSTMで夏目漱石ぽい文章の生成](https://qiita.com/elm200/items/6f84d3a42eebe6c47caa "Title")

### 程序实例（Program Example）

- [Github Link - tv-script-generation](https://github.com/HJTSO/tv-script-generation/blob/master/dlnd_tv_script_generation.ipynb "Title") 

- [Github Link - language-translation](https://github.com/HJTSO/language-translation/blob/master/dlnd_language_translation.ipynb "Title") 

### 参考资料（Reference）

- [Understanding LSTM Networks](http://colah.github.io/posts/2015-08-Understanding-LSTMs/ "Title") 

- [TensorFlow中RNN实现](https://zhuanlan.zhihu.com/p/28196873 "Title") 

- [零基础入门深度学习(5) - 循环神经网络](https://zybuluo.com/hanbingtao/note/541458 "Title") 

- [零基础入门深度学习(6) - 长短时记忆网络](https://zybuluo.com/hanbingtao/note/581764 "Title") 

- [LSTMネットワークの概要](https://qiita.com/KojiOhki/items/89cd7b69a8a6239d67ca "Title") 

- [わかるLSTM](https://qiita.com/t_Signull/items/21b82be280b46f467d1b "Title") 
