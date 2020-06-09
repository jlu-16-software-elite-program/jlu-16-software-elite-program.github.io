---
title: TensorFlow2学习 - 0.Overall & 1.Model
date: 2020-06-09 18:44:04
categories: TensorFlow
tags:
   - TensorFlow
   - Python
   - Deep Learning
---

# {{ title }}


> Q1：学习目标？
>
> A1：了解新版TensorFlow的主要模块，能够完成数据预处理、模型搭建、模型训练、监控等功能的实现
>
> Q2：怎么学？
>
> A2：此前按照一些网课粗略地学习了一些，当前阶段计划从[TF官网的API文档](https://tensorflow.google.cn/api_docs/python/tf)和[Keras的API文档](https://keras.io/api/)中学习
>
> Q3：学习重点？
>
> A3：主要是tf.keras部分的使用
>
> Q4：如何介绍一个class？
>
> A4：
>
> 1. Describe a class with a few words
> 2. list key methods(usage、parameters、return) 
> 3. list key attributes
> 4. how to subclass it?

modules list

- Tensor & opr
- tf.keras
- tf.data
- 
- TensorBoard



## tf.keras

### Content

- Models
- Layers
- Activations
- Callback Funcs
- Data preprocessing
- Optimizers
- Metrics
- Losses
- Datasets
- Backbones

### Models

#### 主要内容

> 1. how to subclass a Model class?
> 2. Sequential
> 3. how to train a model?
> 4. save & load a model

#### tf.keras.Model

##### Description

通常将一个网络模型用Model封装

- 如果比较简单可以用Sequential或Model方法描述输入输出流程

- 如果模型比较复杂会实现一个继承子类



##### Method

- `Model(inputs,outputs,name)`
`tf.keras.Model()`是Model类的构造函数，如果直接用这个构造函数来建立模型，一般需定义`tf.keras.Input`作为inputs，再实现计算过程，将out作为Model的outputs

```python
import tensorflow as tf

inputs = tf.keras.Input(shape=(3,))
x = tf.keras.layers.Dense(4, activation=tf.nn.relu)(inputs)
outputs = tf.keras.layers.Dense(5, activation=tf.nn.softmax)(x)
model = tf.keras.Model(inputs=inputs, outputs=outputs)
```

- `model.summary()`

   输出网络结构（主要是各层的name以及维度），可以选择`print_fn`
   
- `model.get_layer(name = None, index = None)`

   用name或index获取某一层

##### Attribute

- model.weight

   列出模型中所有权值（可训练的+不可训练的）

##### Subclass

继承Model需要实现`__init__`方法和`call`方法

其中`__init__`主要是定义一些layer或者其他设置，并且要显示调用父类的初始化函数`super(MyModel, self).__init__()`

call有两个形参：inputs和training(boolean)，此处实现用inputs计算outputs的过程，返回outputs

```python
import tensorflow as tf

class MyModel(tf.keras.Model):

  def __init__(self):
    super(MyModel, self).__init__()
    self.dense1 = tf.keras.layers.Dense(4, activation=tf.nn.relu)
    self.dense2 = tf.keras.layers.Dense(5, activation=tf.nn.softmax)

  def call(self, inputs):
    x = self.dense1(inputs)
    return self.dense2(x)

model = MyModel()
```

#### tf.keras.Sequential

##### Description

用来封装线性堆叠层的模型，应该继承自Model

```python
model = tf.keras.Sequential()
model.add(tf.keras.layers.Dense(8, input_shape=(16,)))
model.add(tf.keras.layers.Dense(4))
```
##### Method

- `Sequential.add(layer)`

- `Sequential.pop()`

   弹出最后一层

- `Sequential.build()`

   可能add时没有指定输入数据的维度，需要build显示指定一下

#### Training APIs

##### Description

> `compile`
>
> `fit`
>
> `evluate`
>
> `predict`
>
> `train_on_batch` `test_on_batch` `predict_on_batch`

##### Method

- compile

   配置训练过程，此处需要指定：优化器、损失、metric等

   ```python
   Model.compile(
       optimizer="rmsprop",
       loss=None,
       metrics=None,
       loss_weights=None,
       weighted_metrics=None,
       run_eagerly=None,
       **kwargs
   )
   ```

- fit

   进行模型训练，此处需要指定：数据、标注、batch_size、epoch、callback等

   ```python
   Model.fit(
       x=None,
       y=None,
       batch_size=None,
       epochs=1,
       verbose=1,
       callbacks=None,
       validation_split=0.0,
       validation_data=None,
       shuffle=True,
       class_weight=None,
       sample_weight=None,
       initial_epoch=0,
       steps_per_epoch=None,
       validation_steps=None,
       validation_batch_size=None,
       validation_freq=1,
       max_queue_size=10,
       workers=1,
       use_multiprocessing=False,
   )
   ```

   - x：numpy tensor tf.data generator keras.utils.Sequence
   - y：当x中有标注时y可以不用指定
   - shuffle
   - initial_epoch：integer
   - workers：多worker供数据，需要数据是generator或keras.utils.Sequence类型
   - use_multiprocessing：多进程供数据
   - **Return**：返回History 主要记录每一次epoch训练的结果，结果包含loss以及acc的值

- evluate

   通常在fit后做evluate，拿到一些结果指标，默认返回loss和metric组成的list，也可以设置返回dict

   ```python
   Model.evaluate(
       x=None,
       y=None,
       batch_size=None,
       verbose=1,
       sample_weight=None,
       steps=None,
       callbacks=None,
       max_queue_size=10,
       workers=1,
       use_multiprocessing=False,
       return_dict=False,
   )
   ```
   
- `predict`

   如果数据量比较小，可以直接用 `model(x)`, or `model(x, training=False)` 
  
  ```python
  Model.predict(
      x,
      batch_size=None,
      verbose=0,
      steps=None,
      callbacks=None,
      max_queue_size=10,
      workers=1,
      use_multiprocessing=False,
  )
  ```
  
- `train_on_batch` `test_on_batch` `predict_on_batch`

   train or test or predict a single batch

   ```python
   Model.train_on_batch(
       x,
       y=None,
       sample_weight=None,
       class_weight=None,
       reset_metrics=True,
       return_dict=False,
   )
   Model.test_on_batch(
       x, y=None, sample_weight=None, reset_metrics=True, return_dict=False
   )
   Model.predict_on_batch(x)
   ```

#### Save & Load

主要介绍模型的加载与保存

TensorFlow保存格式为hdf5或tf，一般1.x版默认保存格式为h5，2.x版本默认保存为tf，时间+epoch+loss为文件名

##### Mothods

- `Model.save`

   保存网络的拓扑逻辑、参数、优化状态

   ```python
   Model.save(
       filepath,
       overwrite=True,
       include_optimizer=True,
       save_format=None,  # 'tf' or 'h5'
       signatures=None,
       options=None,
   )
   ```

- `tf.keras.models.load_model`

   ```python
   tf.keras.models.load_model(filepath, custom_objects=None, compile=True)
   ```
   
- `clone_model`

   拷贝一个网络

   ```python
   tf.keras.models.clone_model(model, input_tensors=None, clone_function=None)
   ```

- others

   如果有需求比如只初始化网络头部，可以尝试`get_weights`、`set_weights`、`load_weights`等

