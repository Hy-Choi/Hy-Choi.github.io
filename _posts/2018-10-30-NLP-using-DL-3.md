---
layout: post
title: "딥러닝을 통한 자연어 처리 강의 정리 part 3"
description: " edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리, Optimization 부분"
categories: [Lecture]
tags: [edwith, Deeplearning, NLP]
comments : True
redirect_from:
  - /2018/10/30/
---

# How do we optimize the loss function?


- 지도 학습(Supervised Learning)을 하기위해서 우리가 현재 준비된것.
  - 인공 신경망 모델 구조, Loss 정의, graph 구성

![image-20181030171718451](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181030171718451.png)


#### Optimization 하는 두가지 방법

##### 1) Local, iterative optimization

- Hypothesis에서 부터 임의의 1개의 점을 잡아서 Stocastic하게 조금씩 바꿔가며 학습을 진행후, 100개의 노이즈 데이터를 넣어서 가장 작은 포인트를 사용. 이 동작을 반복하여 학습 수행
- 장점 : DAG가 어떤 그래프이던지, Loss function이 무엇이던지 사용할수 있음.
- 단점 : 차원이 커지면서 파라미터가 늘어나면 학습이 잘안됨. 물론 클러스터가 크면 가능함...

##### 2) Gradient-based optimization

- DAG의 모든 노드가 Continuous, differentiationstiable 하다면, 사용 가능
- "함수의 미분한 값은 함수의 방향을 의미한다" 라는 정의를 통해 구현 -> Loss function의 미분을 통해 어느 방향으로 가야 값이 작아지는지 판단
- Learning rate (step size)를 통해 방향으로 욺직이는 크기를 정하고, 이것을 통해 값계산.
  - ![image-20181030172917206](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181030172917206.png)
- 장점 : 방향을 확실히 알수 있다.
- 단점 : Learning rate에 따라서 최적화가 되지 않을 가능성이 있다.



=> 우리는 Gradient-based optimization을 활용하게 되는데, 이것을 어떻게 하면 효율적으로 활용할지 집중.



#### Optimization 하는 두가지 방법

##### -> Gradient를 어떻게 계산 할 것인가?

- 수동적으로 미분하는 것은 DAG가 복잡해 짐에 따라 어려움이 존재
- Automatic differentiation(autograd)
  - Chain rule 사용 -> 복잡한 것일 지라도 단계별로 미분을 하여 곱하여 전체 미분을 구할수 있음.
  - DAG는 Input으로 부터 Composition한 것이기때문에, 쭉 계산으로 진행해나가고 끝에서부터 다시 돌아로면됨! 
  - 과정1 : jacobian-vector product - 한번 계산한 것으로 활용가능
  - 과정2 : DAG의 역순으로 계산 - Loss, 시그모이드 … 순으로

![image-20181030174457610](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181030174457610.png)

- DAG를 그려놓고 나으면, Backend에서 어떤 작업을 하고 있는지 걱정하지 않고 컴파일러가 개발해놓은대로 사용하게 된다.

![image-20181030174850672](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181030174850672.png)



#### Gradient-based Optimization

- Backpropagation을 통해 최적화를 진행을 하면, 파라미터를 업데이트 하게된다. 그 방법으로  Gradient descent, L-bfgs... -> 한번에 모든 데이터에 대해서 하게 되면 파라미터가 커졌을때 동작하기 어려운 문제가 있다.
- 대안으로 Stocastic Gradient Descent.

##### Stocastic Gradient Descent

- 가정은 "전체 비용은 훈련 샘플의 일부를 선택 후 나눠서 계산한 전체 비용의 근사값(Approximate)와 같다." 입니다.  아래의 단계를 반복

1. M개의 훈련 샘플을 선택합니다. 이를 미니배치(Mini batch) 라고 합니다. 

2. 미니배치 경사를 계산합니다.

3. 매개변수를 업데이트합니다. 

4. 검증 세트로 구한 validation loss 가 더 이상 진전이 없을때까지 진행합니다

- Early Stopping : Validation loss가 가장 낮은 순간에 학습을 멈춤
  - 과적합 방지
  - 데이터가 너무 커져서 학습을 전체 다한후에 멈추기 힘들기때문에 활용
  - 일반적으로 100번에서 10,000번 사이에 멈춤
- Learning rate를 정하는 방법에 대한 문제.=> Adaptive Learning rate
  - SGD의 문제 = Variance(노이즈)가 큼
  - Adam, Adadelta 등의 Optimizer가 존재

**본 내용은 edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리한 것이다. 중간에 나오는 자료도 참조하였다.**