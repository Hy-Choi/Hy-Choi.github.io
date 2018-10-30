---
layout: post
title: "딥러닝을 통한 자연어 처리 강의 정리 part 4"
description: " edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리, Summary, Questions 부분"
categories: [Lecture]
tags: [edwith, Deeplearning, NLP]
comments : True
redirect_from:
  - /2018/10/30/
---


# Summary

(본 내용은 edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리한 것이다. 중간에 나오는 자료도 참조하였다.)

- Hypothesis 
  - 신경망의 구조를 디자인하는 것 - DAG(directed acyclec graph)를 만들기
- 최적의 Loss function 결정
  - 신경망 자체를 Distribution을 Output하게 만드는 것 
    - Input에 대하여 Output이 나올 확률을 만들기
  - Negative log probability로 활룡
- DAG와 Loss를 통하여 좋은 모델을 찾는 방법?
  - Gradient-based optimization
    - backpropagation을 통하여 계산
    - 사실 잘몰라도 되지만, Endline에서 어떻게 계산하는지 볼수있음.
  - DAG를 만들어서 할수 있다.
  - SGD를 주로 활용, Adaptive learning rate 활용



#### Question

- 어떤 문제에 따라서 네트워크 구조를 어떻게 결정해야하나요? 이론적인 접근.
  - 인풋과 아웃풋, Distribution이 정확하게 주어지더라도 알수가 없다.
  - 어떤 Operater를 쌓아서 구조를 만들었는지에 따라서 조금 정할순 있지만, 실전에 정할수 있는 효과적인 방법이 없다.
  - 인풋과 아웃풋, 네트워크 구조를 인풋으로 활용하고 모델의 성능을 아웃풋으로 하여 지도 학습을 한다면 만들수 있지 않을까? => Meta learning
- 확률적 경사 하강법(Stochastic Gradient Descent)에서 확률적(Stochastic)은 무엇을 뜻하나요?
  - Deterministic : 계산 할때마다 계속 같은 값이 나오는 것
  - Stochastic : 계산중간에 노이즈나 임의의 값이 중간에 존재하여 계산시 값이 바뀌는것
  - Stochastic gradient라 하는 이유? 모든 학습 데이터를 활용하는 것이 아니라 어떤 학습 데이터를 고르느냐가 달라지기 때문에
- 베르누이분포에서 시그모이드 함수와 소프트맥스 함수를 쓰는게 어떤 차이가 있나요?
  - 실제로 Output은 동일하지만, Gradient 계산에서는 다르다.
- Early stopping을 사용해야하는 이유는 무엇입니까? 실제로 많이 쓰나요?
  - 디버깅에 문제가 존재하지만(SGD자체의 문제 : 큰 차원의 함수를 계산할때 실수를 하면, Loss는 줄지만 방향이 틀린 문제), 버그가 없다면 활용!
  - Reguralization하는 수많은 방법중 한개
- 편향된(bias) 데이터에서 확률적 경사 하강법을 사용할 수 있나요? (Ex. 최신의 데이터가 더 중요하다)
  - SGD의 중점 : 정확한 Gradient를 계산 못하니까 일부를 통해서 근사치 계산
  - 중요한 포인트가 학습 데이터가 독립적인 가정, 질문과 같이 bias한 데이터인 경우에는 어떤 bias인지가 중요
  - 예를 들어, Sampling 방식을 바꾼다. bias의 형태를 알고, 기본적인 몬테 카를로 Sampling이 아닌 다른 방식. 
  - 만약 bias가 어떤지 전혀 모른다면, Validation set을 조심스럽게 정확히 만든다.