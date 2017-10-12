---
layout: post
title:  "Optimization Algorithms"
date:   2017-09-13 10:25:45 +0900
categories: classes improving-deep-neural-networks
---

> ### 시작에 앞 서...
> 이 글은 Coursera의 [Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization](https://www.coursera.org/learn/deep-neural-network) 수업을 정리한 자료입니다.<br/>
> 교수님의 말씀을 잘 이해하지 못한 부분도 있으니, 수업을 들으시면 더욱 좋을 것 같습니다.
>
> - Week1 : [Practical Aspects of Deep Learning]({{ site.baseurl }}{% link _posts/2017-09-08-practical-aspects-of-deep-learning.md %})
> - Week2 : [Optimization Algorithms]({{ site.baseurl }}{% link _posts/2017-09-13-optimization-algorithms.md %})
> - Week3 : [Hyperparameter tuning, Batch Normalization, and Programming Frameworks]({{ site.baseurl }}{% link _posts/2017-09-14-hyperparameter-tuning.md %})
{:.intro}

## Mini-batch gradient descent

Gradient descent를 지금까지 사용해 왔는데요, 모든 train set을 한번에 돌리는 형태로 사용했으니, **batch gradient descent**를 사용한 셈입니다. **Stochastic gradient descent**는 한 번에 하나의 case만 수행하는 경우를 뜻합니다. 다시 말해서, $$W, b$$를 변경하는 시점이 다릅니다. batch의 경우에는 전체를 다 수행하고 결정한다면, stochastic은 case마다 반영하게 됩니다. stochastic은 batch보다 수행이 빠르지만, $$W, b$$가 처리하는 데이타에 따라 편향되기 쉽습니다. 
그래서 그 사이에 적당한 규모로 수행하는 방법은 **mini-batch**라고 합니다. 그러니까, 적당한 묶음으로 gradient descent를 수행하는 방법입니다. 데이타 편향적일 수 있으므로, 먼저 shuffle로 순서를 바꾸고, mini-batch size로 데이타를 partition으로 분할합니다. 분할된 하나의 묶음을 epoch라고 합니다. 그럼 다음에 관계가 성립됩니다.

$$ mini-batch size \times \text{total epoch} = m + remains $$

그리고 epoch에 대한 notation은 `{n}`로 표현합니다. 

$$ X^{\{n\}} = \text{n-th epoch data} $$

## Understanding mini-batch gradient descent

**mini-batch**는 과연 제대로 잘 작동할까요? 전체가 아닌, 부분 집합으로 구해지기 때문에 cost는 변화폭이 있지만 대체적으로 감소하는 경향을 보입니다. mini-batch를 사용하게 되면, size를 결정하는게 중요한 문제가 됩니다. size는 hard-ware의 특성을 고려하여 결정합니다. 그리고 $$2^n$$ 꼴의 수를 결정하는데, 주로 64, 128, 256, 512가 많이 쓰입니다.

## Exponentially weighted averages

기존의 Gradient Descent을 보다 빠르게 작동하게 위해서, Exponentially weighted averages를 살펴보겠습니다. 통계학에서는 Exponentially weighted moving average이라고 합니다.

$$V_t = \beta V_{t-1} + (1 - \beta)\theta_t$$

$$\beta$$는 새로운 hyper parameter로 이전 값의 반영치를 결정하게 됩니다. 예를 들어, $$\beta = 0.9$$라고 한다면, $$V_{t}$$을 구하는 시점에서도 $$V_{t-10}$$의 값이 반영되게 됩니다.  

## Understanding exponentially weighted averages

$$V_t = \beta V_{t-1} + (1 - \beta)\theta_t$$

Exponentially weighted averages의 장점은 하나의 변수만 사용해서 평균값을 관리하므로, 메모리 사용이 효율적이라는 점입니다. 재밌는 점은 $$\beta$$가 1에 가까울 수록, 더 많은 이전 값들을 참조하게 된다는 점입니다. 만약, $$\beta = 0.98$$이라면, $$\frac{1}{1 - \beta} = \frac{1}{1 - 0.98} = 50$이라는 값을 가지게 되므로 결과적으로 이전 50개의 값들을 참조하게 됩니다. 당연한 의문이지만, $$V_{10}$의 경우에는 10번째 항이므로 이전 50개의 항이 없는 상태이므로, 제대로된 값을 가지지 못합니다. 이를 bias라고 표현하며 다음에서 살펴보겠습니다.

## Bias correction in exponentially weighted averages

Exponentially weighted average의 특성상, 시작 단계에서 bias가 나타납니다. 이는 어느 정도 iteration이 지나면, 극복될 수 있는 부분이나, 어느 정도 보정도 가능합니다.

$$ V_{t}^{\text{corrected}} = \frac{V_{t}}{1 - \beta_1^t} \tag{1} $$

만약, t가 2라면, (1)의 식을 이용해서 다음과 같이 구합니다.

$$ 
\begin{align}
V_{2}^{\text{corrected}} & = \frac{V_{2}}{1 - \beta_1^2} \\
& = \frac{V_{2}}{1 - (0.98)^2} \\
& = \frac{V_{2}}{0.0396}
\end{align}
$$

## Gradient descent with momentum

앞서 배웠던, exponentially weighted average를 gradient에 적용하는 걸, **momentum** 혹은 **gradient descent with momentum**이라고 합니다. 일반적인 gradient descent는 optima에 수렴하기 위해서, 여러 step을 거치게 되는데, 마치 진폭 운동처럼 진행하게 됩니다. momentum은 이러한 진폭을 줄이고, 진행방향을 보다 완만하게 하는 역활을 합니다.
기존의 gradient descent에서 $$dW, db$$를 구하고, $$W, b$$에 값을 반영하기 전에 다음의 과정을 추가합니다.

$$ V_{dW} = \beta V_{dW} + (1 - \beta)dW \qquad V_{db} = \beta V_{db} + (1 - \beta)db $$

$$ W = W - \alpha V_{dW} \qquad b = b - \alpha V_{db} $$

설명드린 것처럼, $$\beta V_{dw}$$를 진행하고 있는 방향이라고 한다면, $$(1 - \beta)dW$$는 새로운 방향을 일부 적용한다고 볼 수 있습니다. 즉, 갑작스럽게 방향이 바뀌어서 진폭 운동이 일어나는 일이 없게 되며, 자연스럽게 빠르게 수렴할 수 있게 됩니다.

## RMSprop

**RMSprop**(Root Means Square prop)은 **momentum**과 마찬가지로 gradient descent의 최적화 방법 중에 하나입니다. 이름에서 알 수 있듯이, 제곱값을 구해서 나누는 역활을 합니다.

$$ S_{dW} = \beta S_{dW} + (1 - \beta)dW^2 $$

$$ S_{db} = \beta S_{db} + (1 - \beta)db^2 $$

$$ W = W - \alpha \frac{dW}{\sqrt{S_{dw} + \epsilon}} $$

$$ b = b - \alpha \frac{db}{\sqrt{S_{db} + \epsilon}} $$

수식에서 알 수 있듯이, $$S_{dw}$$, $$S_{db}$$를 바탕으로 $$W, b$$에 상대적으로 적은 값을 더 많이 반영하게 됩니다. 예를 들어, $$S_{db}$$가 큰 값이라고 하면, 실제 $$b$$에 반영될 때에는 조금만 변경되게 됩니다. 주의해야 할 점은, $$S_{db}$$가 0에 가까운 작은 값이라면 $$b$$값이 엄청나게 커질 수 있다는 점입니다. 그래서 $$\epsilon$$을 더해주어, 그러한 오류를 미연에 방지하게 됩니다.

## Adam optimization algorithm

Adam(Adaptive Momentum estimation) optimization algorithm은 momentum과 RMSprop을 동시에 적용하는 방법입니다. 

먼저, momentum과 RMSprop을 구합니다.

$$ V_{dW} = \beta_1 V_{dW} + (1 - \beta_1)dW \qquad V_{db} = \beta_1 V_{db} + (1 - \beta_1)db $$

$$ S_{dW} = \beta_2 S_{dW} + (1 - \beta_2)dW^2 \qquad S_{db} = \beta_2 S_{db} + (1 - \beta_2)db^2 $$

bias correction을 진행합니다.

$$ V_{dw}^{\text{corrected}} = \frac{V_{dw}}{1 - \beta_1^t} \qquad V_{db}^{\text{corrected}} = \frac{V_{db}}{1 - \beta_1^t} $$

$$ S_{dw}^{\text{corrected}} = \frac{S_{dw}}{1 - \beta_2^t} \qquad S_{db}^{\text{corrected}} = \frac{S_{db}}{1 - \beta_2^t} $$

보정된 값으로 $$W, b$$를 변경합니다.

$$ W = W - \alpha \frac{V_{dW}^{\text{corrected}}}{\sqrt{S_{dw}^{\text{corrected}}+\epsilon}} \qquad b = b - \alpha \frac{V_{db}^{\text{corrected}}}{\sqrt{S_{db}^{\text{corrected}}+\epsilon}} $$

앞 서 다 배운 내용의 종합 선물 세트에 가깝습니다. optimization을 위해, 몇 가지 계산을 추가하면서 hyper parameter가 더욱 늘어났습니다. 그래도 $$\alpha$$를 외에는 튜닝이 필요하지 않습니다.

- $$ \alpha $$ : learning rate, 튜닝이 필요합니다.
- $$ \beta_1 $$ : momentum의 계수로 0.9
- $$ \beta_2 $$ : RMSprop의 계수로, 0.999
- $$ \epsilon $$ : RMSprop의 분모가 작은 경우를 위한 보정치로, $$ 10^{-8} $$

## Learning rate decay

**Learning rate**는$$W, b$$에 변경할 때에 사용하는 계수입니다. rate이 크면, 빠르게 학습하지만, 수렴하기 어렵습니다. 반대로, 작은 경우에는 학습이 더디게 진행됩니다. 따라서, learning rate를 조금씩 작게 만들어 간다면 초기에는 왕성한 학습을 하다 수렴에 가까워질수록 조금씩 학습하도록 만들 수 있습니다.

$$ \alpha = \frac{1}{1 + \text{decay-rate} \times \text{epoch-num}}\alpha $$ 

`epoch-num`은 mini-batch에서 수행 횟수입니다. 즉, $$\alpha$$는 수행 횟수가 거듭될 수록, `decay-rate`에 곱한 만큼 감소됩니다. learning rate decay에는 위의 형태 외에도 여러가지가 사용됩니다. 지수나 제곱근을 이용하거나, discrete stair 형태를 사용하기도 합니다. 

## The problem of local optima

Gradient descent는 좋은 알고리즘이지만, local optima(global optima의 반대말로, 전체에서 가장 낮은 J가 아니라 초기값 근처의 optima에서 수렴하는 현상)를 극복하기 어렵다는 문제점이 있습니다. 하지만, neural network 상에서 근복적으로 발생하기 어렵다고 합니다. feaure가 수천에 이르고, training set도 충분히 많다면 local optima는 발생되기 더욱 어렵다는 것이죠. 물론, 미분이 값이 0인 경우가 있기는 하나, local optima가 아닌 saddle point로 밝혀졌습니다. 사실, 진짜 문제는 local optima가 아니라 기울기가 너무 완만해서 gradient descent의 속도가 느려지는 plateau(정체기)입니다. 이런 경우는 Adam과 같은 optimization방법으로 극복할 수 있습니다.
