---
layout: post
title: "딥러닝을 통한 자연어 처리 강의 정리 part 2"
description: " edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리, Loss Function, Probability 부분"
categories: [Lecture]
tags: [edwith, Deeplearning, NLP]
redirect_from:
  - /2018/10/27/

---


### Loss Function

Loss function을 정의하는 연습을 하기위해서 Probability 이론을 짧게 짚고 넘어가고 있다.

- Event set Ω : 세상에서 일어날 수 있는 모든 것이 들어있음.
- Random variable X : 이벤트 셋안의 어떤 이벤트도 취할 수 있는 것.
- Probability : X가 어떤 이벤트를 취할 것인가?
  - Non negative : 확률은 0보다 커야한다.
  - Unit volume : 모든 이벤트가 될 확률을 다 더하면 1이다.
- Joint probability : 두 개의 Random variable X, Y에서 두 이벤트가 동시에 될 확률.
- Condition probability P(Y|X) : X가 ei일때, Y가 ej인 경우.
- Marginal probablitity : Joint Probability에서 한 개가 아예 상관 없는 경우!

![image-20181024175522356](/Users/hyunyoung/Library/Application%20Support/typora-user-images/image-20181024175522356.png)



### Probability를 가지고 Loss function에 적용하자

지도 학습은 주어진 Input에 대하여 Output을 구하는 문제인데, 이 문제를 주어진 Input에 대하여 Ouput이 y'일 확률을 구하는 문제로 바꿔서 푼다.

Bernoulli distribution 

- Sigmoid function의 결과는 0과 1사이의 값을 주기때문에, X가 주어졌을때 Y가 1일 확률을 리턴해주는 모습을 보여준다. 
- 이는 이진 분류 문제를 풀기 위해, Distribution 아웃풋을 만드는 것이다.



Categorical distribution

- 이진 분류가 아닌 다양한 분류를 해결하기 위한 문제
- 파라미터가 다양함. 
- 뉴럴 넷의 아웃풋이 다 더한 값이 1이 되도록 만들고, 각각의 값이 각 클래스가 될 확률이 된다.
- 마지막에 나온 값을 Exp를 해서 Non-negative한 값으로 만들고, 이와 같이 만든 값을 다더한 값을 Nomalization으로 나누면, 더해서 1이된다. => Softmax Function



Gaussian distribution

- Regression 문제이므로 이벤트가 무한대로 많은 경우.
- 사용하지 않으므로 지나감.!



### Loss :  Maximum Likelihood Estimation

- Distribution 문제로 바꾸어 푸는 이유는 이러한 문제인 경우는 적용할 Loss를 바로 찾을 수 있기 때문이다. (Loss function을 만들이유가 없다.) 학습 데이터를 사용한 모델의 아웃풋의 확률이 가장 높아지는 ML 모델을 찾게된다. 이와 같이하면, DAG 가 어떻게 생겼는지 볼 이유가 없다.
- 확률을 최대로 하기때문에, 결과 값에 - 붙여서 최소화 모델로 만들게 된다. 
- Log를 넣은 이유? 확률이 보이면, 로그를 통해 계산한다로 생각해라. Negative function을 Minimize 한다.
- DAG 맨뒤에 Loss Fuction의 Node를 넣어주어 사용. -> Code 재사용 가능
- 예를 들어 아래에 Logistic Regresion을 보자. 궁극적으로 Loss를 계산하는 것까지 DAG를 그린다.

![image-20181024181930281](/Users/hyunyoung/Library/Application%20Support/typora-user-images/image-20181024181930281.png)

**본 내용은 edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리한 것이다. 중간에 나오는 자료도 참조하였다.**

