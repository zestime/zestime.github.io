---
layout: post
title:  "Practical Aspects of Deep Learning"
date:   2017-09-05 14:03:45 +0900
categories: classes improving-deep-neural-networks
---

> ### 시작에 앞 서...
> 이 글은 Coursera의 [Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization](https://www.coursera.org/learn/deep-neural-network) 수업을 정리한 자료입니다.<br/>
> 교수님의 말씀을 잘 이해하지 못한 부분도 있으니, 수업을 들으시면 더욱 좋을 것 같습니다.
>
> - Week1 : [Practical Aspects of Deep Learning]({{ site.baseurl }}{% link _posts/2017-08-29-introduction-deep-learning.md %})
> - Week2 : [Optimization Algorithms]({{ site.baseurl }}{% link _posts/2017-09-01-neural-networks-basics.md %})
> - Week3 : [Hyperparameter tuning, Batch Normalization, and Programming Frameworks]({{ site.baseurl }}{% link _posts/2017-09-03-shallow-neural-network.md %})
{:.intro}


Recognize the importance of initialization in complex neural networks.
Recognize the difference between train/dev/test sets
Diagnose the bias and variance issues in your model
Learn when and how to use regularization methods such as dropout or L2 regularization.
Understand experimental issues in deep learning such as Vanishing or Exploding gradients and learn how to deal with them
Use gradient checking to verify the correctness of your backpropagation implementation

이번 강의는 지난 강의에서 neural network을 더욱 심화하여 성능을 향상시키는 방법에 대해 다루고 있습니다. 그러기위해서 기초적인 몇 가지 개념들을 학습해야 할 필요가 있습니다. train/dev/test 셋들이나, bias와 variance에 대한 이해, 또 regularization에 대한 지식도 필요로 합니다. 그리고 몇 가지 실험적인 이슈를 다루는 것까지가 이번 주에 학습할 내용이 되겠습니다.

# Setting up your Machine Learning Application

## Train / Dev / Test sets

neural network과 같은 알고리즘은 훈련에 사용되는 데이타와 검증에 사용되는 데이타를 따로 구분해서 사용합니다. 만약, 훈련 과정에 사용된 데이타를 가지고 검증을 하려고 하면, 이미 $$W$$와 $$b$$ 값에 반영되었기 때문에 항상 좋은 결과만 보여주게 됩니다. 확보된 전체 데이타에서 각 역활에 맞게 나누어 사용하는데, 이를 train, dev 그리고 test set으로 구분합니다. **train set** 은 neural network을 훈련시키는데 사용되는 데이타입니다. neural network은 훈련된 사용된 데이타와 성능이 어느 정도 비례하기 때문에, 대부분의 데이타를 훈련에 할당합니다. 나머지 조금의 데이타만 남겨 두는데, 예를 들면 0.25%에 해당되는 데이타를 dev나 test로 활용합니다. 예전에는 **dev(cross-validation) set** 과 **test set** 을 구분하여, 알고리즘 자체에 대한 검증 dev set으로 진행하고, test set을 이용해서 정확도를 산출하였습니다. 설명에서 알 수 있듯이, dev와 test의 성격이 조금 겹치는 감이 있기에, 통합하여 사용하는 조직이 많다고 합니다.

## Bias / Variance

**Bias** 와 **Variance** 는 성능을 진단하는 하나의 척도입니다. 그림을 보면 훨씬 직관적인데,

![Bias and Variance]({{ site.url }}/assets/bias_and_variance.png)

쉬운 이해를 위해 logistic regression으로 표현했고 좌측이 high bias인 상태이고, 우측이 high variance인 상황입니다. 좌측 그림은 O,X의 구분을 정확하게 가르기 보다는, 대충 간결한 직선으로 구분짓고 있습니다. 즉, **bias** 는 간략화하여 예측도가 나빠진 상황을 뜻합니다. 우측 그림의 경우는 지나치게 세세하게 구분하는 바람에 복잡하게 구분짓고 있습니다. 이는 high **variance** 인 상태로, 데이타의 복잡도나 노이즈가 그대로 반영되어 오히려 예측도 나빠지는 상황을 뜻합니다. 우측 그림의 세세한 구분 보다는, 중간 그림의 적절한 영역 구분이 예측도가 더 좋다는 뜻이 됩니다.
neural network의 경우는 어떻게 진단을 할 수 있을까요? 그림을 그려서 판단해야 할까요? train error와 dev error를 비교하여 판단합니다. train set과 dev set의 error가 서로 다를텐데, 그 값간의 차이를 바탕으로 bias 와 variance를 진단하게 됩니다. **bias** 라면, train set을 제대로 반영하지 못했기 때문에 train error값이 상대적으로 높게 측정됩니다. **variance** 라면, train data는 잘 반영하지만 본 적이 없는 데이타의 경우 예측이 나쁘기에, dev error가 train error보다 높게 측정됩니다.

## Basic Recipe for Machine Learning

앞 서 bias와 variance를 배웠는데, 그 다음에 우린 어떤 행동을 취해야 할까요? 일단, bias에서 시작합니다. bias는 train set도 제대로 반영하지 못하는 상태이기 때문에, 모델에 문제가 있을 가능성이 높습니다. hidden layer나 node를 조정하여 neural network을 더욱 크게 만든다거나, 훈련 과정을 늘리는 방법을 생각할 수 있습니다. 아니면 데이타를 잘 반영할 수 있는 다른 neural network architecture를 적용할 수도 있습니다. 이를 통해, bias를 낮추고 나면, variance를 확인할 차례입니다. variance는 더 많은 데이타를 통해 완화할 수 있습니다. 또한 정규화를 통해서 데이타의 편향을 조절할 수도 있습니다 이런 과정을 통해, low bias에 low variance인 상태의 최적의 알고리즘을 만들 수 있게 됩니다.

예전에는 bias와 variance가 trade-off 관계에 있었습니다. 하나를 높이면 하나가 줄어드는 그런 상태였으나, neural network에서는 한 쪽을 상쇄하지 않아도 다른 쪽은 효과적으로 제어할 수 있게 되었습니다. 예를 들면, 더 많은 데이타는 bias에 상관없이 variance를 낮출 수 있습니다. 혹은, 더 많은 hidden layer는 variance에 영향없이 bias를 낮출 수 있습니다.


# Regularizing your neural network

## Regularization

Overfitting을 제어하는 방법으로 소개된 Regularization은 cost function을 통해 적용됩니다. 이전 강의에서 살펴본 logistic regression의 cost function J는 다음과 같습니다.

$$ J(w,b) = \frac 1 m \Sigma_{i=1}^m L(\hat{y}^{(i)}, y^{(i)})  $$

여기에, regularization을 적용하면,

$$ J(w,b) = \frac 1 m \Sigma_{i=1}^m L(\hat{y}^{(i)}, y^{(i)}) \color{maroon}{ + \frac{\lambda}{2m} \| w \|_2^2 } $$

$$ \| w \|_2^2 = \Sigma_{j=1}^{n_x} w_j^2 = W^T W \tag 1 $$

즉, $$J$$를 구할 때 마다, $W^T W$을 구해서 반영한다는 뜻입니다. 여기서 사용한 (1)은 2차원의 Euclidean norm(유클리드 노름)으로 다루고 있기 때문에, **L2 Regularization** 이라 부릅니다. 물론, **L1 Regularization**도 있습니다.(1차원의 거리값으로 절대값의 합)

지금까지 logistic regression으로 보았습니다만, 이번에는 neural network을 기준으로 살펴보겠습니다.

$$ J(w^{[1]},b^{[1]},...,w^{[l]},b^{[l]}) = \frac 1 m \Sigma_{i=1}^m L(\hat{y}^{(i)}, y^{(i)}) \color{maroon}{+ \frac{\lambda}{2m} \Sigma_{l=1}^L \| w_F^{[l]} \|^2} $$

$$L$$에서 알 수 있듯이, layer에 대한 처리가 추가되었습니다. 그리고 [Frobenius Form](https://en.wikipedia.org/wiki/Matrix_norm#Frobenius_norm)을 사용한다는 점입니다. matrix norm 중에서 가장 일반적인 경우를 p-norm이라고 하는데, p가 2인 특별한 경우 Frobenius Form이라고 합니다. 아까와 같이 2랑 동일하나, matrix의 경우라 달리 표기한다고 생각하시면 되겠습니다. 이렇게 cost function을 구했다면, gradient descent에 반영할 차례입니다.

$$ dw^{[l]} = \frac{1}{m} dz^{[l]} X^T \color{maroon}{ + \frac{\lambda}{m} W^{[l]}} $$

이러한 regularization을 weight decay라고도 합니다.

## Why regularization reduces overfitting?



## Dropout Regularization
## Understanding Dropout
## Other regularization methods

# Setting up your optimization problem

## Normalizing inputs
## Vanishing / Exploding gradients
## Weight Initialization for Deep Networks
## Numerical approximation of gradients
## Gradient checking
## Gradient Checking Implementation Notes
