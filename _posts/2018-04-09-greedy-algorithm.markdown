---
title: Greedy Algorithm
layout: post
author: "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Algorithm]
---

- 주어진 문제를 풀 때, 각 상황에 맞추어 최선의 선택을 반복해서 정답을 찾아가는 과정이다. Greedy Algorithm 이름에서 Greedy(탐욕)는 전체를 생각하는 것이 아니라 주어진 상황에 대해서 가장 좋은 것으로 여겨지는 것에 대해 욕심을 부리는 것으로 생각해볼 수 있을 것이다. Greedy Algorithm은 항상 정해진 대로 문제를 해결하는 것이 아니라 전체적인 아이디어만 같고 문제의 특성에 따라 다르다.
- 각 단계에서 최선의 선택을 하고 이들의 집합를 해로 보기 때문에 전체를 다 계산하는 것에 비해 **빠른 계산**이 가능하나 **최적해를 보장하지 않는다**는 단점을 가지고 있다.
- 구성
  1. 선택 함수(Selection function) : 현재 단계에서 사용자가 생각하는 기준 하에서 최선의 후보를 선택
  2. 가능성 점검 함수(Feasibility function) : 선택한 후보가 문제의 조건을 만족하여 해가 될 수 있는지 확인
  3. 해 판단함수(Solution function) : 후보를 Solution에 추가했을 때, 문제의 Solution이 완성되었는지 확인
- Dijkstra algorithm과 같이 주어진 그래프에서 최단 경로를 찾는 알고리즘도 역시 Greedy algorithm으로 해석해볼 수 있다.

### Ex) Coin change problem

- 최소한의 동전들을 가지고 주어진 change를 맞추는 문제이다. 직접 모든 가능한 경우들을 살펴보는 것도 하나의 방법이 될 수 있으나, 주어진 동전들 중 가장 가격이 큰 동전부터 가격이 낮은 동전 순으로 살펴보면서 각각의 경우에 최대한 동전을 사용하는 방식으로 Greedy algorithm을 생각해볼 수 있다.

1. **Selection function** : 주어진 동전들을 내림 차순으로 정렬하고 가장 큰 동전을 선택, 몇개까지 가능한지 계산
2. **Feasibility function** : 이미 Selection을 하는 과정에서 해의 가능성을 판단했으므로 필요없다.
3. **Solution function** : 계산한 결과가 원하는 change와 같은지 확인
