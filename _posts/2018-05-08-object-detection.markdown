---
title: Object detection
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [machineLearning, CNN]
---
# Object detection
YOLO 알고리즘을 다루기 이전에 먼저 Object detection에 대해 더 알고 갈 필요가 있다. 보통 머신러닝을 배우고 처음으로 이미지를 다루는 Model을 만들었을 때, 우리는 이미지 자체의 Label을 다루게 된다. 예를 들면, 사진 안에 고양이가 있다면 우리의 Model은 그 사진을 고양이로 Labeling 된 데이터로 인식하게 된다. 이러한 과정을 **Classification**이라고 할 수 있다. 즉, Classification은 주어진 data를 적절한 class로 labeling하는 과정으로 대상의 위치나 회전, 크기 등의 정보는 관계없이 오로지 그 data에 class에 해당하는 content가 있냐 없냐를 다룬다. location에 대한 정보를 함께 다룰 수도 잇는데, 이를 **Localization**이라고 한다.

**Detection**은 위에서 말한 Classification과 Localization을 함께 다루는 것으로 생각해볼 수 있는데, Classification과 가장 큰 차이점은 더이상 하나의 Object만 다루는 것이 아니라 여러 Object를 다루며 각각의 Object들의 Location에 대한 정보를 함께 다룬다는 것에 있다. 

예를 들면, 사람들과 차, 개가 함께 찍힌 사진이 있다면 여기서 이미지에서 가장 Dominant한 부분을 가지고 Image의 사람, 차, 개 중 하나로 Labeling 하는 것을 Classification이라고 할 수 있으며 대상의 위치 정보를 함께 담고 있다면 Classification with Localization. Image 안에 있는 모든 Object들의 class와 location을 다룬다면 Object detection으로 생각할 수 있다.

![Object detection](/img/cnn/object_detection.jpeg)

# Sliding Window
위에서 언급한 Classification의 결과에 Localization을 추가하기 위한 방법 중 많이 쓰이는 방법으로는 **Sliding window**가 있다. 기본적인 개념은 이미지의 부분을 잘라 만약 그 안에 Object가 있다면 부분의 위치와 크기를 함께 저장한다는 것이다.
![Sliding window](/img/cnn/sliding_window_2.png)

위의 그림에서 볼 수 있는 것처럼 Sliding window는  window를 처음부터 끝까지 움직이며 Object의 유무를 확인하게 된다. 이 과정에서는 Object classification을 위해 logistic regression이나 CNN등의 방법등을 이용하며 window의 크기를 바꿔가며 반복하면 효과적으로 이미지에 있는 Object들의 정보를 위치와 함께 파악할 수 있다.(Localization)

하지만 이 방법에는 문제점이 존재하는데, 바로 수많은 Window에 대해 계산을 반복 수행해야 하기 때문에 Computational cost가 크다는 것이다. 이를 해결하기 위해 사용된 것이 바로 **CNN Implementation**이다.

## CNN Implementation of sliding window
CNN Implementation, 이름은 거창하지만 내용 자체는 그렇게 어렵지는 않다. 간단하게 보면 이전에는 Sliding window의 적용을 window를 바꿔가며 계산을 반복 수행했다면 **CNN Implementation은 한번에 window들의 계산을 다 하는 것으로 생각할 수 있다.**(Computation Result를 공유한다) 다음과 같은 이미지를 보면 이를 보다 쉽게 이해할 수 있다.
![Sliding window CONVNET 0](/img/cnn/sliding_window_convnet_0.png)
![Sliding window CONVNET](/img/cnn/sliding_window_convnet.png)

우선은 `14x14x3`의 input data에 대한 ConvNet을 Train 시켰다고 생각해보자. 이후에 만약 `16x16x3`의 input data에 대해 동일한 ConvNet을 적용한다면 마지막 결과물은 기존의 ConvNet 결과와 같이 `(1 x 1 x # Classes)`와는 달리 `(2 x 2 x # Classes)`의 결과를 얻게 된다. 이를 해석하는 것이 중요한데, `(2 x 2 x # Classes)` 각각의 결과물이 처음 input data 각각의 window에 대한 계산 결과로 생각하는 것이다. 

이렇게 CNN Implementation을 한다면 기존의 경우처럼 계산을 계속 반복하는 것이 아니라 중간중간 계산된 결과물을 각각의 window에서 공유하며 사용할 수 있기 때문에 window를 바꿔가며 다시 계산할 필요없이 한번에 계산할 수 있는 것이다. 계산을 공유하는 방식의 CNN Implementation of sliding window는 계산 속도를 월등히 올릴 수 있었으며 **Object detection in real-time** 등을 가능하게 만들었다.

------------
### Ref
1.  Coursera 강의(Andrew Ng-deep Learning(CNN))

### Related
1. [YOLO](/machinelearning/cnn/2018/05/08/yolo-algorithm/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMTQ2ODM4NCwtMTE2MjU5MDU1MSwxNj
gzMTc3MDY3LDU2MTk3MzE2MCwyMDEyMjQ4NTMzLC0zNTc1ODgx
NjksLTEwMjg4MDAwMjksLTQxNTgzODcyNSwxMDYyNTY3Njc4LD
QzMTE4NDc4NiwxMzUzNDY0OTU4LDY0NzI4ODg2NSw5NjMyNDU1
OTRdfQ==
-->