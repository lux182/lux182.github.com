#### 神经网络的数学基础
##### 张量
- 轴的个数
    矩阵有两个轴
- 形状
    沿着每个轴的元素的个数
- 数据类型
    张量中所包含的数据类型
- 张量是矩阵向任意维度的推广
    [注意， 张量的维度(dimension)通常叫作轴(axis)]。    

##### 张量的运算（神经网络的齿轮）
- 逐元素运算
- 广播
    较小的张量添加新的轴，使其ndim与较大的张量相同
    较小的张量沿着新轴重复，使其形状与较大的张量相同
- 点积

##### 梯度（神经网络的引擎）         
- 梯度(gradient)是张量运算的导数。它是导数这一概念向多元函数导数的推广。多元函数 是以张量作为输入的函数
- 随机梯度下降(SGD)
	沿着梯度的反方向更新权重，损失每次都会变小一点
    SGD变体(optimizer)
        动量的概念尤其值得关注，它在 许多变体中都有应用。量解决了 SGD 的两个问题:收敛速度和局部极小点
        
- 反向传播
    链式法则
    f(W1, W2, W3) = a(W1, b(W2, c(W3)))
    (f(g(x)))' = f'(g(x)) * g'(x)
    

#### 神经网络入门
- 神经网络最常见的三种使用场景:二分类问题、多分类问题和标量回归问题。
 将电影评论划分为正面或负面(二分类问题) 
 将新闻按主题分类(多分类问题)
 根据房地产数据估算房屋价格(回归问题)

##### 神经网络主要围绕四个方面
 层，多个层组合成网络(或模型)。
 输入数据和相应的目标。
 损失函数，即用于学习的反馈信号。
 优化器，决定学习过程如何进行。

```
import keras

keras.__version__
from keras.datasets import mnist
from keras import models
from keras import layers
from keras import optimizers
from keras.utils import to_categorical

# 输入图像保存在 float32 格式的 Numpy 张量中，形状分别为 (60000, 784)(训练数据)和 (10000, 784)(测试数据)
(train_images, train_labels), (test_images, test_labels) = mnist.load_data()
train_images = train_images.reshape((60000, 28 * 28))
train_images = train_images.astype('float32') / 255

test_images = test_images.reshape((10000, 28 * 28))
test_images = test_images.astype('float32') / 255

train_labels = to_categorical(train_labels)
test_labels = to_categorical(test_labels)

# 构建网络
# 这个网络包含两个 Dense 层，每层都对输入数据进行一些简单的张量运算， 这些运算都包含权重张量。权重张量是该层的属性，里面保存了网络所学到的知识(knowledge)
model = models.Sequential()
model.add(layers.Dense(32, activation='relu', input_shape=(28 * 28,)))
# 10 路 softmax 层，它将返回一个由 10 个概率值(总和为 1)组成的数组。 每个概率值表示当前数字图像属于 10 个数字类别中某一个的概率
model.add(layers.Dense(10, activation='softmax'))

# 网络的编译
# 现在你明白了，mse 是损失函数，是用于学习权重张量的反馈 信号，在训练阶段应使它最小化。
# 你还知道，减小损失是通过小批量随机梯度下降来实现的。 梯度下降的具体方法由第一个参数给定，即 rmsprop 优化器
model.compile(optimizer=optimizers.RMSprop(lr=0.001), loss='mse', metrics=['accuracy'])
# 下面是训练循环
# 现在你明白在调用 fit 时发生了什么:网络开始在训练数据上进行迭代(每个小批量包含 128 个样本)，共迭代 5 次[在所有训练数据上迭代一次叫作一个轮次(epoch)]。
# 在每次迭代过程中，网络会计算批量损失相对于权重的梯度，并相应地更新权重。5 轮之后，网络进行了 2345 次梯度更新(每轮 469 次)，网络损失值将变得足够小，
# 使得网络能够以很高的精度对手写数字进行分类。
model.fit(train_images, train_labels, epochs=5, batch_size=128)

test_loss, test_acc = model.evaluate(test_images, test_labels)

```