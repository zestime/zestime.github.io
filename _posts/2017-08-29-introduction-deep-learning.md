---
layout: post
title:  "1. Introduction Deep Learning"
date:   2017-08-29 15:49:45 +0900
categories: classes neural-networks-and-deep-learning
---

> ### 시작에 앞 서...
> 이 글은 Coursera의 [Neural Networks and Deep Learning Deep](https://www.coursera.org/learn/neural-networks-deep-learning) 수업을 정리한 자료입니다.<br/>
> 교수님의 말씀을 잘 이해하지 못한 부분도 있으니, 수업을 들으시면 더욱 좋을 것 같습니다.
>
> - Week1 : Introduction Deep Learning
> - Week2 : [Neural Networks Basics]({{ site.baseurl }}{% link _posts/2017-09-01-neural-networks-basics.md %})
> - Week3 : [Shallow Neural Network]({{ site.baseurl }}{% link _posts/2017-09-03-shallow-neural-network.md %})
> - Week4 : [Deep Neural Network]({{ site.baseurl }}{% link _posts/2017-09-05-deep-neural-network.md %})
{:.intro}

neural network의 인기에 힘입어, coursera에 전문 코스가 생겼습니다. Machine Learning 강좌로 유명하신 Andrew Ng 교수님과 함께 배워보도록 하겠습니다.

# Introduction to deep learning

neural network에 대한 정의와 종류 및 트렌드에 대해 논합니다. 그리고 neural network의 인기에 대해 진단합니다.

## What is a neural network?

neural network을 딱딱하게 정의하기 보다는, 간단한 예제를 통해 접근합니다. Machine Learning의 단골 주제인, Housing Price을 예측해 보겠습니다. 아시다시피,고전적인 접근은 집값이 선형(linear)적이다로 보는 것이죠. 집의 크기가 방의 개수 같은 요소(feature)로 집값을 결정하는 방법입니다. 하지만, 거기에는 너무 많은 요소들이 있으며, 요소들이 독립적이지 않다는 것이 현실입니다. 이를 neural network로 표현하자면 이렇습니다.

![Housing Price Prediction]({{ site.url  }}/assets/housing_price_prediction_concept_NN.png)

완전히 반영할 수도 없지만,

여러 가지 특징(feature)를 바탕으로 그것들을 종합하여 가면서, 자연스레 network 구조를 도출합니다. 그리고 그림에서 동그라미를 neuron 혹은 node라고 합니다. neuron은 아시다시피, 판단과 기억에 중요한 역활을 하는 뇌의 구성 요소입니다. 사실, neuron의 정확한 작동을 아직 모르기에, 흉내 정도만 내고 있다고 생각하시면 되겠습니다.

![Housing Price Prediction - ReLU]({{ site.url  }}/assets/housing_price_prediction_ReLU.png)

만약, polynomal regression(다항 선형 회귀)를 이용한다면, 아래와 같이 형태로 구해지게 됩니다.

![Housing Price Prediction - Polynomial regression]({{ site.url  }}/assets/housing_price_prediction_polynomial_regression.png)

예전 강의에서 가져온 자료입니다. feature간의 관계를 나타내기 위해서, Polynomial의 형태를 쓰고 있습니다.

## Supervised Learning with Neural Networks

data성격에 machine learning의 갈래가 나뉘는데, supervised 라는 것은 결과가 명확한 data라는 뜻입니다.
간단히 말하면, data들의 집합 $$X$$가 있을 때, 데이타에 대응하는 결과(치역)에 해당하는 $$Y$$가 있으면 supervised이며, Y가 없으면 unsupervised가 됩니다. unsupervised는 답이 없는 상황에서 학습을 하는 셈입니다.

그리고 X의 성격에 따라 아래와 같이 구분할 수 있습니다.

- Structured
  - 의미를 가진 데이타 ex) 가격, 나이,
  - 정규화되어 있는 값들로 DB의 컬럼
- Unstructered
  - 비정형화된 데이타 ex) 오디오, 픽셀, 텍스트
  - Sequence Data

그리고 neural network의 몇 가지 종류를 언급합니다.

- Standard Neural Network
- CNN(Convolutional Neural Network)
  - 합성곱 신경망
  - 이미지 분석에서 많이 쓰임
- RNN(Recurrent Neural Network)
  - 순환 신경망
  - 음성인식, 언어번역에서 사용


## Why is Deep Learning taking off?

neural network이 이렇게 인기를 가지는 이유가 뭘까요? 잘 작동하기 때문입니다. 너무 당연한 소리인가요? 

![Scale drives deep learning progress]({{ site.url  }}/assets/scale_drives_deep_learning_progress.png)

그래프 하단에 위치하는 algorithm들은 과거에는 잘 나갔으나, 지금은 상단의 neural network에 밀린 녀석들입니다. 여기에는 여러가지 이유가 있는데, 내적 외적 변화가 이루졌기 때문입니다.

- 데이타의 증가
- 계산 성능의 향상
- 알고리즘의 개선

물론, 그렇다고 모든게 순조롭고 아무런 문제없이 AI가 세상을 지배하는 수준으로 발달된 것은 아닙니다. 아직 가야할 길이 많으며, 우리가 믿고 있는 것 또한 전부가 아닐 수 있습니다. 

