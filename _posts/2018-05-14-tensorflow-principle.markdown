---
title: Tensorflow principle
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Machine Learning]
---

### Dataflow graph

텐서 플로우는 **dataflow graph**형식을 따르는데, 이는 기존의 programming 방식과는 다르게 computation 순서가 각각의 operation간의 관계에 따라 결정되는 것이다. 이는 **Parallel compution**에 아주 유용한 개념인데, 각각의 **node**는 연산의 최소 단위를 말하며 **edge**는 연산에 사용되는 data를 표현한다. 예를 들어 두 개의 matrix를 곱해서 하나의 matrix 결과를 만드는 `tf.matmul`를 생각해보자. 이 operation에는 2개의 matrix input을 의미하는 2개의 incoming edge, 연산을 담당하는 node 그리고 matrix multiplication의 결과를 의미하는 1개의 outgoing edge가 존재한다.

![tensorflow dataflow](/img/tf/tensors_flowing.gif)

> - Node : 연산의 최소 단위
> - Edge : 연산에 사용되는 data를 표현

### `tf.Graph`란?

`tf.Graph`에는 tensorflow에서 필요한 2가지 핵심적인 정보를 포함하고 있다. 첫번째는 **Graph Structure**인데, 이는 앞에서 말한 것처럼 Node와 Edge들을 통하여 각각의 Operation이 어떻게 구성되어있는지를 알 수 있게 해준다. 또한 Tensorflow는  `tf.Graph` 안에 meta data의 collection을 저장하는데, 이를 `Graph collection`이라 하며 사용자는 `tf.add_to_collection`, `tf.get_collection`을 통해 key와 함께 object를 추가하거나 확인할 수 있다. <br /><br/>
예를 들어, `tf.Variable`을 만들게 되면 이는 기본적으로 **global variables**와 **trainable variables**에 추가되며 나중에 `tf.train.Saver`나 `tf.train.Optimizer`를 만들 경우 default argument로 사용하게 된다.
<br /><br/>
Tensorflow는 dataflow graph construction phase부터 시작하는데, 이는 node를 의미하는 `tf.Operation`과 edge를 의미하는 `tf.Tensor`를 `tf.Graph`에 추가하는 것을 의미한다. 우리가 따로 지정을 안해도 tensorflow는 **default graph**를 제공하며 기본적으로 Operation들은 default graph에 속한다.

> - `tf.constant(42.0)` : 42.0을 만드는 single `tf.Operation`을 만드며 이를 default graph에 추가한다. 그리고 constant value에 해당하는 `tf.Tensor`를 반환한다.
> - `tf.train.Optimizer.minimize` : default graph에 **gradient**를 계산하는 operation과 tensor를 추가하며 실행했을 때 variable에 gradient를 적용하는 `tf.Operation`를 반환한다.

### Naming operation

`tf.Graph`는 자신이 가지고 있는 `tf.Operation`에 대한 **namespace**를 정의하는데, 기본적으로는 Tensorflow가 각각의 operation이 unique한 이름을 갖게 하며 사용자가 직접 정의해서 보기 편하게 만들수도 있다. 만약에 사용자가 따로 지정하지 않는다면 `tf.Operation`의 결과로 나오는 `tf.Tensor`의 이름은 operation 이름을 따서 정해지며 만약에 같은 이름을 가진 operation이 사전에 존재한다면 `_1`, `_2` 등을 붙혀 구분해준다. 
> - `tf.constant(2)` : Const:0라는 이름을 가진 `tf.Tensor`를 return한다.

위의 예제에서 Const:0으로 뒤에 0이 붙은 것을 확인할 수 있는데, 이는 `tf.Tensor`가 `tf.Operation`의 output 중 몇번째인지 index를 의미한다. 즉, `tf.Tensor`는 다음과 같은 이름 형식을 지닌다.

`<OP_NAME>:<i>`
> - `<OP_NAME>` : operation의 이름
> - `<i>` : operation의 output 중 tensor에 해당하는 index


### `tf.Session`이란?

`tf.Session`은 python program과 같은 **Client program**과 **C++ runtime**간의 connection을 나타내는 class이다. 이는 Tensorflow가 local machine이나 remote device를 효과적으로 사용할 수 있게 하며 Graph에 정보를 cache함으로써 보다 효과적으로 여러번의 계산을 반복할 수 있도록 해준다.
<br />
<br />
`tf.Session`은 사용시에 CPU나 GPU와 같이 Physical resource를 점유하기 때문에 context manager로도 사용될 수 있으며 이는 `with` block과 함께 사용하기에 아주 좋다. 보통 `tf.Session.run` method를 사용하게 되는데 이는 `tf.Operation`을 실행시키거나 `tf.Tensor`를 evaluation하기 위함으로 구조적으로 보면 계산하고자 하는 대상들을 **fetch list**로 설정하여 `tf.Graph`내에서 실행되어야 하는 Operation들을 **subgraph**를 생성하여 실행하며 결국 관련된 fetch list와 관련된 모든 Operation이 실행된다.
<br />
<br />
또한, `tf.Session.run`에는 `feed_dict`라는 option이 있는데, 이는 `tf.placeholder`와 같이 user의 input이 필요한 tensor에 값을 대입해주는 역활을 한다. 이러한 기능을 통해서 User는 구축해놓은 dataflow graph를 input만 바꾸어가며 반복할 수 있게 된다.

<pre><code class="language-python line-numbers"  numbering># 3개의 floating point 값을 포함하는 vector를 예상하는 placeholer 정의
x = tf.placeholder(tf.float32, shape=[3])
y = tf.square(x)

# y에 해당하는 operation을 실행하기 위해서는 x에 해당하는 operation을 실행해야 하는데, 
# 이를 feed dict를 통해 placeholder에 필요한 값을 건네준다.
with tf.Session() as sess:    
    print(sess.run(y, feed_dict={x:[1,2,3]})) # -> [1,4,9]</code></pre>

> - 위와 같은 경우 실행해야 하는 **fetch list**는 y tensor이며 y가 실행되기 위해 필요한 x tensor까지 함께 실행되는 것이다.

### 정리
1. Operation들을 사전에 정의하여 Graph에 추가함으로써 구조 만들기
2. 얻고자 하는 tensor들을 `tf.Session.run`에 argument로 넘기면 Graph 안에서 관련된 계산들이 알아서 진행된다.
    - 사전에 정의된 값들이 아니라 `tf.placeholder`처럼 user의 input에 의존하는 tensor들이 있는데 이 경우에는 `tf.Session.run`의 `feed_dict`를 이용해서 user가 원하는 값을 대입시켜준다.
    - `Operation` 단위로 실행되기 때문에 필요로 하는 `tensor`(edge)가 없다면 관련 계산이 끝날 때까지 알아서 기다리게 되고 이러한 구조는 분산 컴퓨팅에 있어서 최적의 구조라고 생각된다.

### TODO
Multiple graph나 visualization과 같은 정보들 추가하기

### Ref
1. Tensorflow guide(https://www.tensorflow.org/programmers_guide/graphs)