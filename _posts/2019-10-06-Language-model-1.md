---
layout: post
title: Language Model (1) N-Gram, Perplexity
date: 2019-10-06 22:49
categories: [NLP]
author: "JongHyun"
---

### What is language model?

**Language Model은 단어들로 이루어진 문장에 대한 확률을 계산하는 일종의 언어에 대한 Probability Distribution Function**으로 볼 수 있다. 현재의 NLP가 대세가 되기전 가장 주로 사용되었는데 처음에 Model을 구성하고 나면 그 다음부터는 굉장히 빠르게 **문장의 확률을 계산, 현재 문장 다음에 올 단어 예측등**을 할 수 있다. 속도가 굉장히 빠름에도 불구하고 NLP에게 자리를 빼았긴 건 Train Set에 없던 문장들에 대해 **Out of vocabulary (이하 OOV)**가 Smoothing과 같은 해결책에도 불구하고 결국 큰 문제점으로 작용했기 때문이었다. 현재는 NLP가 주로 사용됨에도 불구하고 Langauge Model은 NLP에 부가적으로 자주 사용되기 때문에 꼭 필요한 개념으로 볼 수 있다.

Language Model에 대한 예로 다음과 같은 문장에 대한 확률을 생각해보자.

$$\text{I can play the piano}$$

이 문장에 대한 확률을 계산하기 위해서는 앞에서부터 차례대로 각각의 **조건부 확률**을 계산하고 이를 **Chain Rule**로 묶어주면 된다.

> 1. I can play the 다음에 piano가 나올 확률
> 2. I can play 다음에 the가 나올 확률
> 3. I can 다음에 play가 나올 확률
> 4. I 다음에 can이 나올 확률
> 5. I가 나올 확률

$$P(\text{I, can, play, the, piano}) = \\ P(\text{piano|I, can, play, the})\\ \cdot P(\text{the|I, can, play})\\ \cdot P(\text{play|I,can}) \\ \cdot P(\text{can|I}) \\ \cdot P(\text{I})$$

위의 식을 일반화하면 다음과 같다.

$$P(w_1, \cdots, w_n) = \Pi_i P(w_i|w_1, \cdots, w_{n-1})$$

이제 남은 건 다음과 같이 Trainig Corpus안에서 각 Case에 대한 Count를 세어주고 이를 이용해 조건부 확률 계산하여 곱하는 것 뿐이다.

$$P(\text{piano|I,can,play,the})=\\ \frac{count(\text{I, can, play, the, piano})}{count(\text{I, can, play, the})}$$

굉장히 심플하고 직관적인 방법이지만 이 과정에서는 큰 문제가 존재한다. 만약 우리가 I can play the piano에 대한 확률을 성공적으로 계산할 수 있다고 해보자. 그렇다면, 이를 기반으로 I can play the violin에 대한 확률은 계산할 수 있을까? 이는 불가능한데, **매번 문장의 처음부터 각 단어에 대한 확률을 계산하며 늘려나가기 때문에 I can play the violin에 대해 확률을 계산하려면 무조건 Trainig Corpus에 I can play the violin이 존재해야 한다.** 즉, 위와 같은 방법으로 Language Model을 구성하려면 문장이 구성되는 모든 경우가 Train Set에 있어야 하고 이는 현실적으로 말이 안되는 조건이다.

### N-gram

N-gram은 위의 문제를 해결한 방법인데 먼저 **Markov Process**를 살펴보자. Markov process와 Markov Property를 서술한 Markov Assumption의 정의는 다음과 같다.

> - **Markov Process** : Stochastic Process (Random Process) 중 미래 사건에 대한 Conditional Prbability가 오로지 현재의 State에 의전하는 Markov proprty를 가진 process
> - **Markov Assumption** : Markov property가 가정된 대상을 서술하기 위한 용어이다.

이 개념을 Language Model에 적용해보면 이전처럼 현재 단어 이전의 모든 단어에 대한 조건부 확률을 계산할 필요가 없이 Markov assumption에 따라 현재 단어만 고려하거나 앞의 한개 또는 두개 이상의 단어들에 대해서만 조건부확률을 계산하면 된다. 여기서 **몇개의 단어를 고려하느냐에 따라 N-gram이 정해지게 된다.(보통 Trigram까지가 자주 쓰인다)**

- Unigram : 현재의 단어만 고려

$$P(w_1, \cdots, w_n) = \Pi_i P(w_i)$$

- Bigram : 이전 단어 1개를 함께 고려

$$P(w_1, \cdots, w_n) = \Pi_i P(w_i|w_{i-1})$$

- Trigram : 이전 단어 2개를 함께 고려

$$P(w_1, \cdots, w_n) = \Pi_i P(w_i|w_{i-1}, w_{i-2})$$

앞에서 다룬 I can play the piano에 대한 확률을 Bigram model을 통해 계산하면 다음과 같다.

$$P(\text{I, can, play, the, piano}) = P(\text{piano|the})\\ \cdot P(\text{the|play})\\ \cdot P(\text{play|can}) \\ \cdot P(\text{can|I}) \\ \cdot P(\text{I})$$

이제는 함께 고려하는 단어의 수가 줄었기 때문에 적당한 경우의 수에 대해 충분한 통계를 얻을 수 있다. 만약에 Bigram에서 I can play the 다음에 violin이 올 확률을 구하고 싶다면 앞의 한 단어만 고려하기에 다음과 같이 쉽게 계산할 수 있다.

$$P(\text{violin} | \text{I, can, play, the}) \\= P(\text{violin} | \text{the}) = \frac{\text{count(the violin)}}{\text{count(the)}}$$

N-gram을 통해 Language Model을 만드는 과정은 다음과 같다.

1. Train set에서 전체 단어를 통한 Vocab 구성
2. Vocab의 각 단어들에 대한 Co-occurence matrix 생성
   - ex) Vocab에 Hello, world가 있다면 Hello가 Hello와 사용된 횟수, world와 사용된 횟수, world가 Hello와 사용된 횟수, world와 world가 사용된 횟수
   - Unigram의 경우는 이를 구성할 필요없이 각 단어들의 Count만 계산
   - 위 예시는 Bigram의 경우이며 Trigram이상의 Model에 대해서는 함께 고려하는 단어의 수가 하나씩 많아진다.
3. Co-occurence matrix를 Normalize하여 조건부 확률 계산

### Estimation of Language Model

Language Model은 Unigram, Bigram, Trigram등 여러 가지 Model이 존재할 수 있는데 이들을 평가하는 방법으로는 크게 2가지 방법이 존재한다.

1. **Extrinsic** : **특정한 Task**에 대해 End-to-End로 돌려보는 것으로 확실히 비교할 수 있지만 Task Cost가 너무 크다.
2. **Intrinsic** : **Model만으로 바로 비교**할 수 있는 척도를 계산하는 것으로 **application에 무관하게 성능을 평가**할 수 있다.

#### Train set, dev set, test set

Intrinsic Method를 적용하기 위해서는 먼저 Train set과 구별된 (held-out) set을 얻을 필요가 있다. 모델 비교를 보다 객관적으로 하기 위함이며 보통 이 set을 test set이라고 한다. Intrinsic method를 이용하여 모델을 비교하는 과정은 다음과 같다.

1. 여러 모델들을 Train set을 이용하여 구성
2. 각 모델들을 Train set과 구별된 별도의 Test set에 대한 확률 계산
3. Intrinsic Method 척도에 따라 최적의 모델 선별

하지만 이 방법은 자칫 잘못하면 Test set에만 최적화된 Model을 얻을 수 있기 때문에 중간 검토 set인 dev set을 만들어 가장 적절한 모델을 만들고 최종 비교를 test set로 하기도 한다.

#### Perplexity

Perplexity(PP)은 Instrinc method 중 가장 대표적인 method로 **얼마나 확률 분포가 데이터를 잘 설명하는지에 대한 값**으로 **문장의 길이로 정규화된 문장 확률의 역수**로 계산하며 그 값이 작을수록 (확률의 값이 클수록) 좋은 모델로 고려한다.

$$PP(W) = P(w_1,\cdots, w_N)^{-\frac{1}{N}} \\= (\Pi_i P(w_i|w_{i-k}, w_{i-1}))^{-\frac{1}{N}} \text{(For k-gram)}$$

Perplexity는 다음 Post에서 다시 다루겠지만 언어의 **Weighted average branching-factor, 다시 말해 평균적으로 아무 단어에나 뒤따라 나타날 수 있는 단어의 수로 해석**해볼 수 있다. 이를 알 수 있는 좋은 예로 주사위를 들 수 있다. 만약 1~6까지의 숫자 6개로 이루어진 문장에 대한 Perplexity를 계산해보면 다음과 같다. 각 단어의 뒤에 나올 수 있는 단어 경우의 수가 6이라는 걸 생각해보면 branching factor에 대한 이해가 보다 잘 될 수 있을 것이다.

$$PP(dice) = (\frac{1}{6}\cdot\frac{1}{6}\cdot\frac{1}{6}\cdot\frac{1}{6}\cdot\frac{1}{6}\cdot\frac{1}{6})^{-\frac{1}{6}} = 6$$

Perplexity를 test set에 대해 계산하는 과정은 다음과 같다.

1. Test set을 하나의 긴 문장으로 잇는다.
2. 각 문장을 구분하기 위해 문장의 앞과 뒤에 `<s>`, `<s />`를 추가
3. 문장 전체에 대해 Perplexity를 계산

### Language Model의 한계

Language Model은 보통 Train set에 전적으로 의존하기 때문에 만약 **Test set이 다른 Domain이거나 dialect를 사용한다면 기존의 모델은 잘 학습되었더라도 형편없게 사용될 수 있다.** 물론, 이는 Domain이나 dialect를 통일하면 어느정도 해결할 수 있겠지만 더 큰 문제는 따로 있다.

N-gram이더라도 항상 Train set에 모든 경우의 수가 있는 것이 아니기 때문에 Co-occurence matrix에는 count가 0인 sparsity problem이 존재한다. 만약 새로운 데이터에 대해 확률을 계산하는 과정에서 해당하는 값에 대한 통계가 없으면 division by zero가 되어 Language Model을 적용할 수 없게 된다. 이를 해결하기 위한 방법 중 대표적인 방법으로는 count가 0인 Matrix 값에 대해 1을 더해주어 확률을 0이 되지 않게 만드는 Smoothing 기법이 있다.

다음 Post에서는 여러가지 Smoothing기법들과 정보학적 관점에서의 Perplexity에 대해 다뤄보겠다.
