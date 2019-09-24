---
title: tensorflow estimator
date: 2019-07-21 15:29:27
tags:
---
## Estimator
A high-level API that simplify training, evaluation, prediction and export for serving. Base class:  `tf.estimator.Estimator `. It provides the following benefits:
1. Scalable (run in local host or distributed env) without modification of the source code;
2. Built on `tf.keras.layers`, which simplifies customization;
3. Many tasks handled automatically by Estimator, such as `Session` & `Graph`.


## Workflow of tensorflow esitmator

There are mainly three components for tensorflow estimator: model_fn, input_fn and estimator.


```python
def model_fn(features, labels, mode):
    """
    This function does all the network computation
    
    # `features`: input features
    # `labels`: labels for responding example
    # `mode`: training, evaluation or prediction

    # Return: `tf.estimator.EstimatorSpec`
    """
    
```
```python
# `tf.estimator.input.numpy_input_fn
def numpy_input_fn(x, y=None, 
                batch_size=128,
                num_epochs=1, 
                shuffle=None, 
                queue_capacity=100, 
                num_threads=1):
    """

    Return: (features, labels). Features is a dict.
    """
```

The work flow
```python
# model_fn: 
image_classifier = tf.estimator.Estimator(model_fn=model_fn, model_dir=model_dir)

train_input_fn = tf.esitmator.inputs.numpy_input_fn(x=train_data,
                                                    y=train_labels,
                                                    batch_size=64,
                                                    num_epochs=1,
                                                    shuffle=True)

for _ in range(NUM_EPOCHS):
    image_classifier.train(input_fn=train_input_fn)
```

## Estimator

`Estimator`, `model_fn`, `input_fn`是三个主要成分。
`Estimator`相当于一个controller，控制在三种不同的情况下(train, predict, evaluation)的工作流。

```python
tf.estimator.Estimator(
    model_fn, 
    model_dir=None, 
    config=None, 
    params=None, 
    warm_start_from=None)
```
- model_fn: 见下方
- model_dir： checkpoint路径，如果是已经存在的，则模型会load并restore from the checkpoint.
- config: `tf.estimator.RunConfig`
- params: hyperparameters, type of dict. 如batch_size
- warm_start_from: 



```python
def model_fn(features, labels, mode, paramas=None, config=None)

```
- Features, labels是input_fn的返回值
- mode是`train`, `eval` or `predict`
- params: 可以没有该参数；如果有的话，是`Estimator`中的该参数，用于配置hyperparameters
- config：可以没有该参数；如果有的话，是`Estimator`中的config参数，Instance of `tf.estimator.RunConfig`


```python
tf.estimator.RunConfig(
    model_dir=None, 
    tf_random_seed=None, 
    save_summary_steps=100, 
    save_checkpoints_steps=<object object at 0x7efe9a76ca80>, 
    save_checkpoints_secs=<object object at 0x7efe9a76ca80>, 
    session_config=None, 
    keep_checkpoint_max=5, 
    keep_checkpoint_every_n_hours=10000, 
    log_step_count_steps=100, 
    train_distribute=None, 
    device_fn=None, 
    protocol=None, 
    eval_distribute=None, 
    experimental_distribute=None, 
    experimental_max_worker_delay_secs=None)
```

跟分布式训练相关的参数都是通过环境变量`os.environ['TF_CONFIG']`来设置的。