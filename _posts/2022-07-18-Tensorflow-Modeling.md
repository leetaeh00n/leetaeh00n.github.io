---
layout: single
title: "Modeling 세가지 방법"
category: DLM
tag: [python, DeepLearning, Tensorflow]
toc: true
toc_sticky: true
---


# Tensorflow-Model 구현

* Sequential
* Functional
* Subclassing


```python
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras import activations
```

## Sequential
* 직관적인 모델을 빠르게 구현할 수 있다.


```python
def sequential_model(input_shape):
    model = keras.Sequential(
        [   ## Input
            layers.Input(input_shape),
            
            ## 1st conv
            layers.Conv2D(64, 3, strides=1, activation='relu', padding='same'),
            layers.Conv2D(64, 3, strides=1, activation='relu', padding='same'),
            layers.MaxPool2D(),
            layers.BatchNormalization(),
            layers.Dropout(0.5),
            
            ## 2nd conv
            layers.Conv2D(128, 3, strides=1, activation='relu', padding='same'),
            layers.Conv2D(128, 3, strides=1, activation='relu', padding='same'),
            layers.MaxPool2D(),
            layers.BatchNormalization(),
            layers.Dropout(0.3),
            
            ## Classifier
            layers.GlobalMaxPool2D(),
            layers.Dense(128, activation='relu'),
            layers.Dense(1, activation='sigmoid')
        ]
    )
    return model

input_shape = (256,256,3)
model = sequential_model(input_shape)

model.summary()
```

    WARNING:tensorflow:Please add `keras.layers.InputLayer` instead of `keras.Input` to Sequential model. `keras.Input` is intended to be used by Functional model.
    Model: "sequential_2"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv2d_8 (Conv2D)            (None, 256, 256, 64)      1792      
    _________________________________________________________________
    conv2d_9 (Conv2D)            (None, 256, 256, 64)      36928     
    _________________________________________________________________
    max_pooling2d_4 (MaxPooling2 (None, 128, 128, 64)      0         
    _________________________________________________________________
    batch_normalization_4 (Batch (None, 128, 128, 64)      256       
    _________________________________________________________________
    dropout_4 (Dropout)          (None, 128, 128, 64)      0         
    _________________________________________________________________
    conv2d_10 (Conv2D)           (None, 128, 128, 128)     73856     
    _________________________________________________________________
    conv2d_11 (Conv2D)           (None, 128, 128, 128)     147584    
    _________________________________________________________________
    max_pooling2d_5 (MaxPooling2 (None, 64, 64, 128)       0         
    _________________________________________________________________
    batch_normalization_5 (Batch (None, 64, 64, 128)       512       
    _________________________________________________________________
    dropout_5 (Dropout)          (None, 64, 64, 128)       0         
    _________________________________________________________________
    global_max_pooling2d_2 (Glob (None, 128)               0         
    _________________________________________________________________
    dense_4 (Dense)              (None, 128)               16512     
    _________________________________________________________________
    dense_5 (Dense)              (None, 1)                 129       
    =================================================================
    Total params: 277,569
    Trainable params: 277,185
    Non-trainable params: 384
    _________________________________________________________________
    

## Functional
* 무난한 방법


```python
def functional_model(input_shape):
    inputs = keras.Input(input_shape)
    
    ## 1st conv
    x = layers.Conv2D(64, 3, strides=1, activation='relu', padding='same')(inputs)
    x = layers.Conv2D(64, 3, strides=1, activation='relu', padding='same')(x)
    x = layers.MaxPool2D()(x)
    x = layers.BatchNormalization()(x)
    x = layers.Dropout(0.5)(x)
    ## 2nd conv
    x = layers.Conv2D(128, 3, strides=1, activation='relu', padding='same')(x)
    x = layers.Conv2D(128, 3, strides=1, activation='relu', padding='same')(x)
    x = layers.MaxPool2D()(x)
    x = layers.BatchNormalization()(x)
    x = layers.Dropout(0.3)(x)
    ## Classifier
    x = layers.GlobalMaxPool2D()(x)
    x = layers.Dense(128, activation='relu')(x)
    outputs = layers.Dense(1, activation='sigmoid')(x)
    
    model = keras.Model(inputs, outputs)
    return model

input_shape = (256, 256, 3)
model = functional_model(input_shape)
model.summary()
```

    Model: "model"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    input_4 (InputLayer)         [(None, 256, 256, 3)]     0         
    _________________________________________________________________
    conv2d_12 (Conv2D)           (None, 256, 256, 64)      1792      
    _________________________________________________________________
    conv2d_13 (Conv2D)           (None, 256, 256, 64)      36928     
    _________________________________________________________________
    max_pooling2d_6 (MaxPooling2 (None, 128, 128, 64)      0         
    _________________________________________________________________
    batch_normalization_6 (Batch (None, 128, 128, 64)      256       
    _________________________________________________________________
    dropout_6 (Dropout)          (None, 128, 128, 64)      0         
    _________________________________________________________________
    conv2d_14 (Conv2D)           (None, 128, 128, 128)     73856     
    _________________________________________________________________
    conv2d_15 (Conv2D)           (None, 128, 128, 128)     147584    
    _________________________________________________________________
    max_pooling2d_7 (MaxPooling2 (None, 64, 64, 128)       0         
    _________________________________________________________________
    batch_normalization_7 (Batch (None, 64, 64, 128)       512       
    _________________________________________________________________
    dropout_7 (Dropout)          (None, 64, 64, 128)       0         
    _________________________________________________________________
    global_max_pooling2d_3 (Glob (None, 128)               0         
    _________________________________________________________________
    dense_6 (Dense)              (None, 128)               16512     
    _________________________________________________________________
    dense_7 (Dense)              (None, 1)                 129       
    =================================================================
    Total params: 277,569
    Trainable params: 277,185
    Non-trainable params: 384
    _________________________________________________________________
    

## Subclassing
* pytorch와 구현이 비슷하다.


```python
class SimpleCNN(keras.Model):
    def __init__(self):
        super(SimpleCNN, self).__init__()
        
        self.conv_block1 = keras.Sequential(
            [
                layers.Conv2D(64, 3, strides=1, activation='relu', padding='same'),
                layers.Conv2D(64, 3, strides=1, activation='relu', padding='same'),
                layers.MaxPool2D(),
                layers.BatchNormalization(),
                layers.Dropout(0.5),
            ], name = 'conv_block1'
        )
        self.conv_block2 = keras.Sequential(
            [
                layers.Conv2D(128, 3, strides=1, activation='relu', padding='same'),
                layers.Conv2D(128, 3, strides=1, activation='relu', padding='same'),
                layers.MaxPool2D(),
                layers.BatchNormalization(),
                layers.Dropout(0.3),
            ], name = 'conv_block2'
        )
        self.classifier = keras.Sequential(
            [
                layers.GlobalMaxPool2D(),
                layers.Dense(128, activation='relu'),
                layers.Dense(1, activation='sigmoid')
            ], name = 'classifier'
        )
    def call(self, input_tensor, training=False):
        x = self.conv_block1(input_tensor)
        x = self.conv_block2(x)
        return self.classifier(x)

input_shape = (None, 256, 256, 3)
model = SimpleCNN()
model.build(input_shape)

model.summary()
```

    Model: "simple_cnn_1"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv_block1 (Sequential)     (None, 128, 128, 64)      38976     
    _________________________________________________________________
    conv_block2 (Sequential)     (None, 64, 64, 128)       221952    
    _________________________________________________________________
    classifier (Sequential)      (None, 1)                 16641     
    =================================================================
    Total params: 277,569
    Trainable params: 277,185
    Non-trainable params: 384
    _________________________________________________________________
    
