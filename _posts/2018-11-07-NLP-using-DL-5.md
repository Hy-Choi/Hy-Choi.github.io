---
layout: post
title: "딥러닝을 통한 자연어 처리 강의 정리 part 5"
description: " edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리, Text Classification & Sentence Representation"
categories: [Lecture]
tags: [edwith, Deeplearning, NLP]
comments : True
redirect_from:
  - /2018/11/07/
---



# Text classification - Overview

Text Classification을 하다보면, 저절로 Sentence Representation이 연결되게 되어 좋은 문제이다.



#### Text Classification

- 입력 : 자연어 문장, 문단
- 출력 : 입력 텍스트의 카테고리



#### How to represent a sentence

keyword - tokens, Vocabulary, Encoding, Continous vector space

- 인간이 사용하는 말은 굉장히 arbitrary하다. 언어는 이미 abstraction 되어 있기에 그냥 token으로 만들기 어렵다.
- token은 단어, 글자든 뭐든 될수 있다. 원하는데로 나누자. 아래는 예시

![5-1](https://i.imgur.com/oj11BMa.png)

- 이미 만들어 놓은 Vocaburay에서 Token의 Index를 찾아서 문장을 Integer로 변경하게 된다. 아래의 예시
  - (커넥트, 재단에서, 강의, 중, 입니다, .)
  - (5241, 20, 288, 12, 19, 5)



#### How to represent a token

- token은 index라는 한개의 integer로 표현
- token의 표현은 one-hot-vector로 표현
  - one-hot-vector는 vocaburary 만큼의 크기를 가지고, index의 위치만 1이고 나머지 모든 원소는 0을 가지는 벡터
  - Arbitrary를 해결하기 위해서 이와 같이 표현
- 결국 DL을 통해서 분류 문제를 해결하려면, 각 token 마다 Continuous한 벡터를 가지고 있음. 이 벡터는 매트릭스로 표현되는데 즉, one-hot-encoding의 index의에 맞는 row또는 column이 해당 token의 실제 벡터 값이라 보면된다. (= Embedding, Table lookup)



**token에 대한 의미를 가지게 되는 vector를 찾는것 = Table lookup**

**문장에 대한 의미를 가지게 되는 vector를 찾는 것 ?**

 

### 문장을 표현하는 방법

#### CBOW (Continuous bag-of-words)

- 토큰 순서에 상관없이, 문장에 있는 단어들을 100으로 보고, Table lookup을 통해 vector로 변환되어 있는 각단어가 모인 문장의 Average를 통해 표현 - Average 포인트가 문장 표현
- Generalizable to bag-of-n-grams
  - 토큰을 한개씩 보는 것이아니라 n개씩 묶어서 보는 방법
- 문장 분류를 할때 매우 잘됨. CBOW를 후, layer를 쌓아서 분류를 하면 좋은 성능을 보임 
- 문장 분류의 가장 기본적인 모델 - FastText라는 Tool사용 !
- ![5-2](https://i.imgur.com/khauRk7.png)
  - 예를 들어, 문장의 감정 분류의 경우 - 문장 내의 Positive 단어와 Negative 단어의 수에 의해 평균값이 결정되어 좋은 분류가됨
- 결국엔 내가 푸는 문제에 대해서 적합한 Representation을 찾아야한다



#### Relation Network

- Idea : Skip Bigrams - 토큰을 두개 보는데 중간을 띄어서 보는 것
  - 단어들의 모든 페어를 만들고, 이를 신경망으로 만들어서 페어의 Representation을 찾게됨
  - ![5-3](https://i.imgur.com/SjdIf6J.png)
  - 이렇게 만들어낸 Vector를 평균!
  - 물론, 2개씩 페어가 아닌, 3개, 4개도 가능하다
  - ![5-4](https://i.imgur.com/yYX5uyE.png)
- CBOW와의 차이는 Order를 가지고 있고, Pair를 통해 표현하는 것
- 왜? 전체의 Pair를 사용하는가 ? -> Convolutionals Networks
- 장점 : 여러단어의 표현을 탐지!
- 단점 : 무조건 모든 단어를 사용



#### Convolutional Networks

- 이미지가 아닌 Text에서는 1-D convolutional layer고, k-gram을 통해 계산, 이를 hierarchically하게 구조를 만들게 된다.
- Convolution layer가 쌓여 감으로써 Token 끼리 페어를 만들고 -> Multi-word가 합침 -> phrases -> sentence로 계산을함
- 장점 : 좁은 지역의 단어를 잘보게 되고, 다양한 프레임워크에 구현되어있다.
- 단점 : 아주 긴거리의 중요한 Dependency가 있다면, 레이어를 많이 쌓아야만 의미가 생긴다.



#### Self Attention

- Idea : 필요한 경우에만 페어를 만들자
  - RN : t번째 단어의 경우, 모든 단어와 페어
  - CNN : t번째 단어의 경우, 주변의 단어와 페어
  - CNN을 중심 단어로 부터 멀어지면서 가중치가 낮아지는 방법으로 pair를 만든다고 보면, RN은 모든 단어와 가중치가 1인 방법으로 볼수있다.
    - 가중치를 Neural network가 계산해서 넣을수 있을까?
    - 중요한 경우에만 사용하도록
- 가중치의 계산을 또다른 Network를 만들어서 사용, 알파 함수
- ![5-5](https://i.imgur.com/MOs3mjD.png)
- 알파 함수의 결과를 0에서 1사이의 값을 가지도록하거나, 합쳤을때 1이되는 값으로 결정(Softmax)
- 단점 : 계산 복잡도가 높음(매번 모든 페어에 대해 계산), Counting이 어렵다.



#### RNN

- 문장을 읽으면서 한개의 벡터로 만들어 내는 것
- 메모리를 가지고 있고, 1개의 토큰을 읽고 메모리 업데이트를 반복 - 문장을 다읽고 난뒤의 값이 문장을 표현한것
- 문장이 길어졌을때 계산이 커지므로, 양쪽에서 계산하는 Bidirectional RNN이 존재
- 단점 : Parallelized 가 되지 않는 문제 - 속도가 느림
- LSTM, GRU를 통해서 다양한 문제 해결



#### Summary

- 문장 표현 : CBOW, RN, CNN, Self-attention, RNN
- CBOW를 제외한 다른 모델은 문장이 주어졌을때, 벡터 한개가 주어지는게 아니라 토큰 위치별로 모두 벡터가 계속 나오게 됨
- 5가지 방법을 복합적으로 사용가능



- Token representation
  - 학습때 사용되는 것이 Continuous word embedding
- Sentence representation
  - token을 합쳐서 표현
  - CBOW, RN, CNN, Self-attention, RNN 5가지 모델



#### Questions

- 단어 임베딩의 다이어(polysemy) 문제 해결?
  - Polysemy 문제 
  - 단어를 표현하는 Space가 굉장히 큰 차원을 사용 - 다양한 의미를 포함가능, 의미는 다른 단어와 같이 나오기 때문에
  - 여기서 어떤 것을 고르느냐? 라는 문제는 Sentence representation에서 해결 - 앞뒤의 페어를 통해서!
- 학습 데이터에 없는 새로운 단어 표현 방법?
  - 먼저 token 정의 수준이 중요하다 (word?, Character?)
  - 방법 1 : 새로운 단어를 쓰는 데이터를 학습데이터에 추가
  - 방법 2 : 새로운 단어가 들어왔을때 기존에 존재하는 단어를 통해서 벡터를 생성
- 분류 모델 훈련 완료후, 새로운 클래스가 등장했을 때 어떻게 해결하나요?
  - 새로운 모델을 직접 만드는 것이 아니라 저절로 확장시키는 법?
  - 방법 1 : 새로운 클래스가 추가될때 데이터를 모아서 새로 모델 학습
  - 방법 2 : 새 클래스에 대해서 적은 데이터를 Weight 벡터를 그것만 한개 추가하여 학습 -> 원하는 파라미터 노드만 업데이트가 가능하기 때문
  - 방법 3 : 클래스가 굉장히 많고, 각 의미를 가지고 있다면, 클래스의 Description을 통해 클래스 간의 관계를 구해서 만들어 추가
  -  => Zero shot, Few shot
- 임베딩에서 “가깝다”는 벡터 공간에서 코사인 유사도를 말하는 건가요? 아니면 다른 distance metrics 를 정의해서 사용하나요?
  - Token이 비슷하다는 것을 결정한다는 것 : 어떤 네트워크를 통해 학습이 된것이 중요함, 그래서 다양한 Metrics가 사용
  - 경험적으로 대부분 코사인 유사도가 좋다고함.
- Capsule network로 텍스트 분류 문제를 사용하는건 어떤가요?
  - Capsule network의 목표가 중요한 것을 계속 아웃풋에서 유지해야하는데, 텍스트에서는 중요한 부분이 무엇인지 알수없다.
- Attention 모델에서 전체 문장을 보지 않고 고정된 윈도우 사이즈를 정해서 하면 어떤가요?
  - 문장의 길이가 길어졌을때, 속도문제가 있기때문에 윈도우를 써서 줄여서 하기도 한다. 
  - 정답을 이야기하기는 어렵고, 때에 따라 다르다
- Self attention은 어떻게 최적화 되나요?
  - 일반적인 Optimizer 사용 가능
  - Backpropagtion은 Loss function으로 부터 각 노드가 어떤 영향을 주는지 전파 되며 계산하는데, 예를 들어 어떠한 Weight가 긍정적인 영향을 주면 값을 올리고, 부정적인 영향을 주면 값을 줄이는 것이다.
  - Parameter 초기화가 제대로 되지않으면 어떤 Weight가 긍정적이고 부정적인지 판단이 안되어서 학습이 안되는 문제가 있다
  - Parameter 초기화가 까다롭지만, 최근에 Self-attention을 이용할때 어떤 Parameter가 중요한지 나온 논문 존재



**본 내용은 edwith에 조경현 교수님께서 진행하신 딥러닝을 통한 자연어 처리 강의를 정리한 것이다. 중간에 나오는 자료도 참조하였다.**