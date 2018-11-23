---
title: Machine Learning 01 - 基于卷积神经网络的肺部组织切片分类
date: 2017-11-06 21:15:30
categories: Machine Learning
---
# 基于卷积神经网络的肺部组织切片分类 #

<!--more-->

# 前言 #

接到老师的一个项目, 要用卷积神经网络对肺部组织切片进行分类, 但是炸酱面眉头一皱, 发现事情并不单纯.

## 卷积神经网络简介 ##

![CNN](https://www.kernix.com/doc/data/cnn.png)

卷积神经网络 `CNN (Convolutional Neural Network)` 是近些年逐步兴起的一种人工神经网络结构, 因为利用卷积神经网络在图像和语音识别方面能够给出更优预测结果, 这一种技术也被广泛的传播可应用.卷积神经网络最常被应用的方面是计算机的图像识别, 不过因为不断地创新, 它也被应用在视频分析, 自然语言处理, 药物发现, 等等.近期最火的 `Alpha Go`, 让计算机看懂围棋, 同样也是有运用到这门技术.

推荐一下周沫凡的 `CNN` 教学视频: [莫烦: CNN简介](https://morvanzhou.github.io/tutorials/machine-learning/ML-intro/2-2-CNN/).

## 迁移学习 ##

如果我们从头开始训练一个复杂的神经网络是十分耗费时间和精力的, 因为首先要获取到符合要求的海量数据, 同时还需要性能极强的集群来对模型进行训练, 甚至还有必不可少的玄学调参.其实在机器学习领域, 花费半年甚至更久的时间来调参优化模型是很正常的.

所以基于已经训练好的模型参数来进行 `fine-tuning` 后应用到新的模型上是一个省时省力的方案, 也被称之为迁移学习.大部分数据是存在相关性的, 在图片分类问题中, 即便现有模型不包含我们想要的分类, 也可以利用已经训练好的权重来进行fine-tuning, 使其对新的类别进行分类.

详细内容可以看这篇文章: [Building powerful image classification models using very little data](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html)

## 框架选择 ##

目前主流的机器学习框架有: `TensorFlow, Theano, Torch, Caaffee, Keras` 等等.

![Keras](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRWvVwErx6A6k3AzpMc927cfeaMCytCojRYDBnt-UMQVbMjVuRq)

`Keras` 是一个高层神经网络 `API`, Keras 由纯 `Python` 编写而成并基与 `Tensorflow、Theano` 以及 `CNTK` 后端, 并有一下特点:

简易和快速的原型设计（ `keras` 具有高度模块化, 极简, 和可扩充特性）

支持 `CNN` 和 `RNN`, 或二者的结合

无缝 `CPU` 和 `GPU` 切换

{% blockquote %}

无论是哪种框架, 几乎都是基于分布式设计的思想, 先描述出计算图, 然后再向图中填充数据流, 使其运转起来, 最后得到结果.虽然是使用 `Python` 语言来描述计算图, 但是真正繁重的工作都会提交给底层的后端去处理.但这样也给 debug 带来了困难, 因为描述计算图的时候并不能得到数据结果, 只能检查出数据格式是否匹配.

Keras的底层库使用Theano或TensorFlow, 这两个库也称为Keras的后端。无论是Theano还是TensorFlow, 都是一个“符号式”的库。

因此, 这也使得Keras的编程与传统的 `Python` 代码有所差别。笼统的说, 符号主义的计算首先定义各种变量, 然后建立一个“计算图”, 计算图规定了各个变量之间的计算关系。建立好的计算图需要编译以确定其内部细节, 然而, 此时的计算图还是一个“空壳子”, 里面没有任何实际的数据, 只有当你把需要运算的输入放进去后, 才能在整个模型中形成数据流, 从而形成输出值。

就像用管道搭建供水系统, 当你在拼水管的时候, 里面是没有水的。只有所有的管子都接完了, 才能送水。

Keras的模型搭建形式就是这种方法, 在你搭建Keras模型完毕后, 你的模型就是一个空壳子, 只有实际生成可调用的函数后（K.function）, 输入数据, 才会形成真正的数据流。

使用计算图的语言, 如Theano, 以难以调试而闻名, 当Keras的Debug进入Theano这个层次时, 往往也令人头痛。没有经验的开发者很难直观的感受到计算图到底在干些什么。尽管很让人头痛, 但大多数的深度学习框架使用的都是符号计算这一套方法, 因为符号计算能够提供关键的计算优化、自动求导等功能。

引自 http://keras-cn.readthedocs.io/en/latest/for_beginners/concepts/
{% endblockquote %}

这里我们使用 `Keras`, 推荐官方文档: [Keras Documentation](https://keras.io/), 中文文档: [Keas 中文文档](http://keras-cn.readthedocs.io/en/latest/)

## 模型选择 ##

`Keras` 的应用模块 `Application` 提供了带有预训练权重的 `Keras` 模型, 这些模型可以用来进行预测、特征提取和 `fine-tune`.

应用于图像分类的模型, 权重训练自ImageNet:

- Xception
- VGG16
- VGG19
- ResNet50
- InceptionV3
- MobileNet

![Model](https://blog.altoros.com/wp-content/uploads/2017/04/keras-based-image-classifier-with-tensorflow-as-a-back-end-pre-trained-networks.jpg)

起初老师推荐我使用 `ResNet` 152层模型,但是在大量的训练后出现了十分严重的过拟合现象,个人推测是由于训练样本过小(1000-2000张), 而网络的层数过多.

后来在学习了 [yulingtianxia’s blog](http://yulingtianxia.com/blog/2017/05/30/Implementing-CNN-with-MPS/) 后,选择了 `Inception V3` 模型.

# Inception V3 pre-trained network #

![Inceptino V3](https://4.bp.blogspot.com/-TMOLlkJBxms/Vt3HQXpE2cI/AAAAAAAAA8E/7X7XRFOY6Xo/s1600/image03.png)

## 模型搭建 ##

首先我们要采用 `ImageNet` 作为模型基础, 同时去掉最后一层全连接层, 也就是上图中的 `Final part`:

```python
base_model = applications.InceptionV3(weights='imagenet', include_top=False, input_tensor=input_tensor)
```

然后我们要实现自己的 `Final part`, 以对图像的四分类为例:

```python
top_model = Sequential()
top_model.add(Flatten(input_shape=base_model.output_shape[1:]))
top_model.add(Dense(256, activation='relu'))
top_model.add(Dropout(0.5))
top_model.add(Dense(4, activation='softmax'))
```

- `Drop`: 简单来说,`Drop` 就是在每次训练时, 随机休眠部分神经元, 使得权值的更新不再依赖于有固定关系隐含节点的共同作用,阻止了某些特征仅仅在其它特定特征下才有效果的情况, 降低了过拟合出现的概率.
- `Flatten` 层: 将多维输入压平变为一维.
- `Dense` 层: 将学到的“分布式特征表示”映射到样本标记空间的作用, 通俗来说就是一个”分类器”.

推荐一下 [Dropout简单理解](http://www.cnblogs.com/tornadomeet/p/3258122.html) 和 [全连接层的作用](https://www.zhihu.com/question/41037974/answer/150522307) .

`Keras` 训练集设置非常简单, 只要将各类图像放到各个文件夹, 就可以实现对图像的导入.此外, 我们通过生成器实现对图像的规格化和数据增强:

```python
train_datagen = ImageDataGenerator(
        rescale=1./255,
        rotation_range=40,
        width_shift_range=0.2,
        height_shift_range=0.2,
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True,
        fill_mode='nearest')

test_datagen = ImageDataGenerator(rescale=1. / 255)

train_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(img_height, img_width),
    batch_size=batch_size,
    class_mode='categorical')

validation_generator = test_datagen.flow_from_directory(
    validation_data_dir,
    target_size=(img_height, img_width),
    batch_size=batch_size,
    class_mode='categorical')
```

我们以这种方式存放数据集即可:

![directory](https://github.com/Snakeflute/Image-Classification/blob/master/blog_src/directory.png?raw=true)

通过设置生成器中的参数, 可以实现对数据随机性的增强, 降低过拟合.

关于图像预处理, 可以参考 [Image Preprocessing](https://keras.io/preprocessing/image/).

在机器学习中减少 `loss` 提升准确率常用的方法就是梯度下降法, 实际应用中使用 `mini-batch` 梯度下降法来平衡计算性能和 `loss` 收敛效果, 进行模型训练.

```python
model.compile(loss='categorical_crossentropy',
              optimizer=optimizers.SGD(lr=1e-4, momentum=0.9),
              metrics=['accuracy'])

model.fit_generator(
    train_generator,
    steps_per_epoch=nb_train_samples,
    epochs=epochs,
    validation_data=validation_generator,
    validation_steps=nb_validation_samples)
```

模型训练的源码在这里: [finetune_inceptionv3.py](https://github.com/Snakeflute/Image-Classification/blob/master/finetune_inceptionv3.py)

蹭了老师的 **`GeForce GTX 1070`** , 经过一晚上的训练, 效果还是比较好的:

![train_result](https://github.com/Snakeflute/Image-Classification/blob/master/blog_src/train_result..jpg?raw=true)

## 模型预测 ##

模型训练完当然要有 `save` 和 `reload` 辣, 保存的时候只需要一行代码 `model.save`，再给它加一个名字就可以用 `h5` 的格式保存起来就好了:

```python
model.save(full_model_path)
```

导入也很简单:

```python
model = load_model(full_model_path)
```

当然模型还有别的方式可以存储, 这里就不赘述了, 详细的方法可以参考: [Save & reload 保存提取](https://morvanzhou.github.io/tutorials/machine-learning/keras/3-1-save/).

预测的结果好像还不错( 逃:

![predict_batch_process](https://github.com/Snakeflute/Image-Classification/blob/master/blog_src/predict_batch_process.png?raw=true)

![predict_visiualization.png](https://github.com/Snakeflute/Image-Classification/blob/master/blog_src/predict_visiualization.png?raw=true)

# 最后 #

边学边做, 四天时间从无到有, 对自己确实也是很大的挑战, 当然做完也很有成就感. 当然大部分也是借鉴的大牛们的博客和官方文档, 做的还十分简陋, 博客写的也很naive, 若有不妥之处, 望不吝赐教.

最后还要感谢 kkun, 正是他的无私帮助让我多走了很多弯路, 也没学习到了很多知识.