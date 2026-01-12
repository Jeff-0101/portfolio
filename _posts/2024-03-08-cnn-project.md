---
layout: post
title: 卷積神經網路 (CNN) 影像辨識專題
author: Jeff-0101
categories: [深度學習, 影像辨識, Python]
tags: [CNN, TensorFlow, Keras, MNIST]
---

# 卷積神經網路 (CNN) 影像辨識專題報告

這份報告詳細記錄了我對卷積神經網路 (CNN) 的學習與實作過程。

## 專案目標
* 了解 CNN 的基本架構與運作原理。
* 使用 Python 和 TensorFlow 建立一個 CNN 模型。
* 讓模型能夠辨識手寫數字圖像 (MNIST 資料集)。

## 實作內容

### 1. 理論基礎
* 卷積層 (Convolutional Layer)
* 池化層 (Pooling Layer)
* 全連接層 (Fully Connected Layer)
* 激活函數 (Activation Functions)

### 2. 資料準備
我們使用了經典的 MNIST 手寫數字資料集，包含 60,000 張訓練圖片和 10,000 張測試圖片。

### 3. 模型建構
```python
import tensorflow as tf
from tensorflow.keras import layers, models

model = models.Sequential([
    layers.Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(10, activation='softmax')
])
model.summary()
