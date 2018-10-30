---
layout: post
title: "딥러닝을 통한 자연어 처리 강의 정리 part 1"
description: " edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리, Introduction, Overview, Hypothesis sets 부분"
categories: [Lecture]
tags: [edwith, Deeplearning, NLP]
redirect_from:
  - /2018/10/26/
---


### Introduction, Overview, Hypothesis sets 3강좌 정리

(본 내용은 edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리한 것이다. 중간에 나오는 자료도 참조하였다.)

최근 머신러닝의 트렌드는 "궁극적으로 다양한 머신러닝 기법에서 지금 하는 것은 슈퍼바이즈드 러닝 문제로 어떻게 바꾸는 것인가"이다.

![image-20181021210726960](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181021210726960.png)

- 전통적 : 문제에 대해 인풋과 아웃풋의 형태 정확히 나오고, 그것의 절차를 알때에 고민해서 만드는 것이 알고리즘.

- 머신러닝 : specification이 정확하지 않고, 애매하는데, 대신 인풋과 아웃풋의 페어가 주어진 상황에서 예시들을 통하여 거꾸로 알고리즘을 찾아내는 것.

![image-20181021211157031](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181021211157031.png)

- N개의 인풋과 아웃풋으로 구성된 학습 셋이 주어지어 있고. 머신러닝 모델(M)이 인풋을 받아서 아웃풋을 주는 함수(l)와 검증 셋과 실험 셋까지 제공. 검증 셋과 실험 셋은 주어진 데이터가 아닌 거에 대해 올바른 모델을 만들기위해.

- 처음으로 해야할것. Hypothesis(ex. SVM, KNN 등) 과 Optimization algorithm을 선택.

![image-20181021211607243](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181021211607243.png)

현재 까지 결정된것 - 학습/검증/실험 데이터 셋, 함수, hypothesis, 최적화 알고리즘.

- Training - 이때에 학습 셋을 통해 각 Hypothesis에서의 최적의 모델들을 생성

- Model Selection -  학습된 최적의 모델중에서 최고의 모델을 선택. 이 선택과정은 검증 셋을 사용.

- Reporting - 찾아낸 최적의 모델이 잘되는지 실험 셋을 통해 실전에서 잘되는지 확인.



### Hypothesis

일반적 문제 해결을 위한 Hypothesis

- Classification (분류) : SVM, naive bayes, Logistic Regression ...
- Regression : 인풋에 대해 아웃풋이 값을 맞춰야하는 문제 : Linear regression ... 
  - 또한 이안에서 각각 Hyper parameter를 정해야한다.



Neural Networks에서의 Hypothesis

- 네트워크의 구조 
- 구조에 따라서 Hypothesis 안에서의 Weight parameter : 너무 많기 때문에 정하기가 어렵다. 

![image-20181024173051783](/Users/hyunyoung/Library/Application Support/typora-user-images/image-20181024173051783.png)

방향성을 따라서 진행하면 결과를 얻게 됨. 사이클이 존재하지 않음. (Parameter = 변수)

위와 같이 그래프로 만들게 되면 변수, 오퍼레이터와 같은 것을 노드로 구현하기에 객체지향으로 코딩되고, 재사용 가능성이 높다! -> 인공지능 논문이 많이 나오는 이유.
