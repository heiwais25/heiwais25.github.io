---
layout: post
title: Language Model (3) Entropy, Perplexity derivation
date: 2019-10-13 11:01
categories: [NLP]
author: "JongHyun"
---

#### Language Model Series Post

1. [N-gram, Perplexity](/nlp/2019/10/06/language-model-1/)
2. [Smoothing Method](/nlp/2019/10/06/Language-model-2/)
3. **Entropy, Perplexity derivation**

---

Language Model 첫번째 Post에서 다루었던 Perplexity의 정의는 다음과 같다.

$$PP(W) = P(w_1,\cdots, w_N)^{-\frac{1}{N}}$$

확률의 역수를 단어의 수로 Normalize한 형태인데, 사실 왜 Perplexity가 이 형태인지는 언급하지 않았고 그냥 Perplexity의 정의가 이러하다라는 느낌으로 지나갔다. Perplexity의 개념을 보다 자세히 이해하고 식을 유도하기 위해서는 정보학적인 관점에서 접근해야 한다. 이번 Post에서는 Entropy에서 시작하여 Perplexity의 유도로 마무리해보겠다.

### Entropy

Entropy는 정보에 대한 측정 방법으로 측정하고자 하는 변수 $$X$$(word, letters, ...)에 대한 Entropy $$H(X)$$는 다음과 같이 구할 수 있다.

$$H(X)=-\Sigma_{x\in X}p(x)\log_2p(x)$$

**Entropy를 이해하는 Intuitive way는 측정하고자 하는 변수에 대한 정보(distribution, model,...)가 있을 때 이를 평균적으로 어느정도의 값으로 표현할 수 있느냐에 대한 값으로 보는 것**이다. 특히, Log의 base가 2라면 Entropy의 값을 bit 단위로 표현할 수가 있게 된다.

Coin toss를 한 번 예로 들어보자. Fair coin이라면 앞(H)과 뒤(T)가 나올 확률을 0.5씩일 것이다. 그렇다면 이 Coin에 대한 정보를 표현하기 위해 필요한 bit는 몇일까? 경우의 수가 단 2개이며 각각이 같은 확률이기에 0,1을 나타낼 수 있는 1bit만 있어도 될 것 같다. 이를 한번 Entropy를 적용해서 한 번 풀어보아도 역시 1bit가 나오는 걸 확인할 수 있다.

$$
\begin{align}
H(coin) &= -p(H)\log_2p(H) -p(T)\log_2P(T) \cr
        &= -0.5\log_20.5 -0.5\log_20.5 \cr
        &= 1
\end{align}
$$

### Entropy of Language

Coin의 값과 같은 간단한 변수에 대해서는 방금 전처럼 쉽게 Entropy를 계산하였지만 여러 단어들로 이루어진 문장의 Entropy는 어떻게 구할 수 있을까?

기본적으로 문장의 Entropy를 구하는 것은 문장의 길이를 정하고 해당 길이 안에서 문장을 구성하는 단어들의 경우의 수를 고려하여야 한다. 언어 L에 대해서 길이기 n인 문장에 대한 Entropy는 다음과 같이 구할 수 있다.

$$H(w_1, \cdots, w_n)=-\Sigma_{w\in L}p(w)\log p(w)$$

Coin과는 달리 이제는 Entropy가 문장의 길이에 Dependent하기 때문에 이에 대한 효과를 배제하고 보기 위해서는 다음과 같이 **Entropy를 문장의 길이로 나누어주어 Entropy rate**를 사용할 필요가 있다.

$$\text{Entropy rate : }\space \frac{1}{n}H(w_1, \cdots, w_n)=-\frac{1}{n}\Sigma_{w\in L}p(w)\log p(w)$$

Entropy rate를 이용하면 우리가 다루는 문장들을 포함하는 Language에 대한 Entropy를 구할 수 있게 된다. **Language에 대한 Entropy는 길이를 정의할 수 없기에 무한한 길이에 대한 Entropy rate로 생각해볼 수 있다.**

$$H(L)=\lim_{n\rightarrow\infty}-\frac{1}{n}H(w_1, \cdots, w_n)=\lim_{n\rightarrow\infty}-\frac{1}{n}\Sigma_{w\in L}p(w)\log p(w)$$

하지만 문장의 모든 경우의 수를 고려하고 단어들의 True probability를 알아야 하기에 Language에 대한 Entropy는 불가능하다고 볼 수 있는데, 다행히도 **Shannon-McMillan-Breiman theorem**와 **cross entropy**에 의해 해결할 수 있다.

#### Stationary and ergodic process

이 theorem에 따르면 **언어가 Stationary와 Ergodic인 경우** Language의 Entropy는 다음과 같이 계산할 수 있다.

$$H(L)=\lim_{n\rightarrow\infty} -\frac{1}{n}p(w_1,\cdots,w_n)$$

즉, 이전 Entropy의 계산처럼 전체 경우의 수를 고려해야 하는게 아니라 **하나의 긴 문장에는 이미 보다 짧은 문장들의 정보가 포함되어 있기 때문에 하나의 긴 문장만 고려해도 된다**는 것이다. 이제는 기존의 Entropy보다 훨씬 간단한 Entropy를 얻을 수 있게 되었는데, Theorem의 전제로 삼은 stationary, ergodic이 신경쓰일 수 있다.

보통 **Stochastic process (random process)의 경우 확률 함수가 시간에 무관한 경우 Stationary라고 하며 시간에 따른 평균이 확률 공간에서의 평균과과 같으면 Ergodic**이라고 할 수 있다. 쉽게 말해서 Random process가 확률이 시간에 따라 바뀌지 않으면 Stationary, ergodic process라고 할 수 있다. **Language model에서의 n gram의 경우 항상 새로운 확률은 앞에서의 단어에만 의존할 뿐, 시간은 고려하지 않기 때문**에 Language는 Stationary ergodic process라고 볼 수 있다. 물론 실제 Language는 앞에서의 단어에만 의존하는 것이 아니라 시간의 경과에 따라 바뀔 수도 있기에 완벽히 Stationary, ergodic process라고 할 수는 없으나 적당한 Error가 있더라도 충분히 계산적인 이점을 얻을 수 있기에 Language를 stationary ergodic process라고 하는 것이다.

#### Cross entropy

앞에서 다룬 Coin의 경우 Fair하다는 가정을 했기에, 우리는 쉽게 Coin의 실제 확률을 알 수 있었고 Entropy도 또한 구할 수 있었다. 하지만, Language처럼 경우의 수가 너무 다양하고 실제 Statistics를 알 수 없는 경우 단어들의 True probability는 구하기가 불가능하다고 볼 수 있다. Cross entropy는 이렇게 Entropy는 구하고 싶지만 True probability는 알 수 없는 경우에 아주 유용한 Metric이다. True probability가 p이고 우리가 가지고 있는 data로 부터 얻은 model probability가 m인 경우의 Cross entropy H(p,m)의 정의는 다음과 같다.

$$H(p,m) = \Sigma_{w} p(w)\log m(w)$$

물론 data로 부터 얻은 probability distribution m은 True probability와 차이가 있기에 다음과 같이 실제 Variable에 대한 Entropy보다는 항상 크거나 같다. 이는 어찌보면 당연한 결과인데, **우리는 실제 Probability를 모르기 때문에 우리가 추측한 model probability m으로 variable information을 해석하기 위해서는 더 많은 양의 bit가 필요한 것이다.**

$$H(p)\le H(p,m) $$

Cross entropy가 Entropy H(p)의 값에 가까울 수록 variable의 modeling을 더 잘한 것으로 볼 수 있으며 그 척도는 다음과 같이 구할 수 있다.

$$\text{How well accurate model : }\space|H(p,m) - H(p)|$$

실제 **Variable의 Entropy를 구함에 있어서 Cross entropy로 대체하면 역시 어느정도의 오차가 존재하지만 data로부터 얻은 model probability를 통해 entropy를 구할 수 있기에 오차를 감수하고 entropy를 계산하기 위해 대신 사용**한다.

앞에서 다룬 Stationary ergodic process와 Cross entropy를 적용한 Language의 Entropy는 다음과 같이 구할 수 있다.

$$
\begin{align}
H(L) \sim H(p,m) &= \lim_{n\rightarrow\infty}\Sigma_{w \in L} -\frac{1}{n}p(w_1,\cdots,w_n)\log_2 m(w_1,\cdots,w_n)\cr
        &\sim \lim_{n\rightarrow\infty}-\frac{1}{n}\log_2 m(w_1,\cdots,w_n)
\end{align}
$$

### Perplexity Derivation

이제 Language의 Entropy를 구할 수 있게 되었으므로 Language의 Entropy구하고 이로부터 Perplexity를 유도해보자. 먼저 Language의 Entropy는 꼭 무한대가 아니더라도 충분히 긴 단어들의 문장을 사용하면 아래의 식처럼 entropy를 근사값으로 구할 수 있을 것이다. (여기서부터는 다시 우리가 구한 Model probability m을 편의상 p로 쓰겠다)

$$H(W)\sim-\frac{1}{n}\log_2 p(w_1, \cdots, w_n)$$

Entropy는 대상 Variable의 정보를 표현하기 위해 필요한 bit의 수로 볼 수 있다고 했으므로 Entropy로부터 Variable의 정보의 크기를 나타내기 위해서는 다음과 같이 할 수 있을 것이다.

$$
\begin{align}
2^{H(W)} &= 2^{-\frac{1}{n}\log_2 p(w_1, \cdots, w_n)} \cr
        &= 2^{\log_2 p(w_1, \cdots, w_n)^{-\frac{1}{n}}} \cr
        &= p(w_1, \cdots, w_n)^{-\frac{1}{n}} \cr
        &= PP(W)
\end{align}
$$

Variable을 나타내는 정보의 양을 Entropy로부터 계산하고 이로부터 자연스럽게 Perplexity를 유도할 수 있었는데, 이는 Perplexity를 다음과 같이 해석해볼 수 있을 것이다.

> **Perplexity : Train set과 같은 data로부터 얻은 Model probability를 통해 Language를 해석했을 때 필요한 정보의 양**

Cross entropy와 entropy의 관계에서 보았듯이 **같은 대상을 해석하는 데 있어서 필요한 정보의 양은 적으면 적을수록 우수한 model이라고 할 수 있기에 첫 post에서 언급했던 perplexity가 낮을 수록 우수한 model이라는 말도 자연스럽게 이해가 될 수 있다.**

---

이번 Post에서는 Language의 Entropy의 정의와 Entropy를 계산하기 위해 필요한 방법들을 다루었고 마지막으로 Entropy로부터 Perplexity를 유도하였다. 비록 Language model이 최근 들어 주류를 이루고 있는 것은 아니지만 NLP와 계속해서 함께 사용되고 있기에 Language processing이 어떤 길을 걸어왔는지 공부하는 것은 큰 도움이 될 것이다.

1. [N-gram, Perplexity](/nlp/2019/10/06/language-model-1/)
2. [Smoothing Method](/nlp/2019/10/06/Language-model-2/)
3. **Entropy, Perplexity derivation**

#### 출처

1. [Stanford Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/)
