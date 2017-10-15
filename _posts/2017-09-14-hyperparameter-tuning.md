---
layout: post
title:  "3. Hyperparameter tuning"
date:   2017-09-14 10:25:45 +0900
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

# Hyperparameter tuning

## Tuning process

Hyperparameter가 많다는 사실에 놀라셨겠지만, 그걸 또 튜닝해야 한다는 사실에 구름을 걷는 기분이 드실 것 같네요. 많은 hyperparameter를 한 번에 결정할 수 있는 방법은 없으며, 익히 알려진 기본값들도 의심의 여지가 있다고 할 수 있습니다.
예전에는 이런 hyperparameter에 대해서, grid적인 접근(각 feature의 값을 일정 비율씩 조정하는 방법)을 시도했습니다. hyperparameter를 결정하는 일은, 여러 input이 있는 함수와 같기 때문에 각 input을 조금씩 변경하여 그 때의 성능을 측정한 것이죠. 그런데 hyperparameter간의 관계가 비례하는 것이 아니기 때문에, 사실상 의미가 없다고 합니다. 오히려, random한 값으로 탐색하는 것이 더 빠르게 적절한 hyperparameter를 찾을 수 있었다고 합니다.
상대적으로 좋은 성능을 내는, random hyperparameter 조합을 찾았다면, random한 영역을 그 값을 중심으로(coarse to fine) 다시 살펴보는 것이 더 세밀한 성능 향상을 꾀할 수 있다고 합니다.

## Using an appropriate scale to pick hyperparameters

hyperarameter를 random하게 설정하는 경우, 단순히 직선상(linear)의 값으로 생각해서는 안됩니다. 예를 들어, $$\alpha$$의 값이 0.0001과 0.1 사이에 있다고 하면, 그 사이의 random한 값은 0.05를 기준으로 생성될 수 있습니다. 사실, 우리가 원하는 값은 0.0001에서 작은 변화에 초점을 맞춰야 하고, 어느 정도 커진 0.08이나 0.1은 이미 적당히 큰 수로 세세한 값들이 필요 없다면 $$\log$$의 성격을 이용할 필요가 있습니다.

```
a_exp = -4 * np.random.rand()
a = 10 ** a_exp
```

코드에서 보는 것처럼, 지수적으로 접근하여 세세한 값을 더욱 잘 반영할 수 있게 되었습니다. exponentially weighted averages의 $$\beta$$도 마찬가지인데, 0.99와 0.999가 가지는 의미가 상당히 크기 때문에, 이 역시도 직선상의 random한 값이 아닌 $$log$$적인 접근이 필요합니다.

## Hyperparameters tuning in practice: Pandas vs. Caviar

결과적으로, 최적의 hyperparameter를 찾는 일은 여러번에 걸쳐 수행하고 결과를 반영하는 방법외에는 없습니다. 달리 말하면, 일을 잘하기 위한 또 다른 일을 하는 셈입니다. 어쨋든, 이런 상황에서 우리가 취할 수 있는 방법은 크게 2가지입니다.

![Panda vs. Cavior](asset/panda_vs_cavior.png)

왼쪽은 **babysit**으로 분류된 방법은, 하나의 모델을 가지고 점진적으로 개선해 가는 전략입니다. 물론, 성능이 나빠지면 이전으로 되돌리면서 한 걸음, 한 걸음 나아가는 형태입니다. 가용할 자원이 부족한 경우 적절한 방법이 될 수 있습니다. 우측은 **parallel**에서 알 수 있듯이, 자원이 충분하다면 각각의 모델을 개선하기 보다는, 가능한 모델을 다 돌려 보는 것이죠. 전자가 됐든, 후자가 됐든, 장님이 코끼리 만지는 꼴인걸 마찬가지입니다.
 

# Batch Normalization

## Normalizing activations in a network

지난 주에, Normalization에 대해 배웠는데요, $$X$$에 대해서, 평균과 분산을 이용해서 분포를 고르게 하는데 그 목적이 있었습니다. **Batch Normalization**은 동일한 작업을 hidden layer에서도 진행하는 방법입니다. 아시다시피, 각 neuron은 $$Z^{[l]}, A^{[l]}$$을 구하는 linear funciton과 actication function의 조합입니다. linear function의 결과인 $$Z^{[l]}$$에 normalization을 적용해 $$\tilde{Z^{[l]}}을 구하고, 그 결과를 $$a^{[l]}$$의 입력으로 사용하게 됩니다. 이를 통해서, neural network의 train 과정을 더욱 쉽고 잘 작동하게 만들어 줍니다.

기억하시겠지만, **normalization**은 $$\mu$$(평균)과 $$\sigma^2$$(분산)을 $$X$$에 적용하는 방법이었습니다. 마찬가지로, hidden layer의 중간값인 $$Z^{[l]}$$에도 적용하고, 한 가지 작업을 더 하게 됩니다. $$\gamma, \beta$$라는 learnable parameters를 적용해 주어야 합니다.

$$ \mu = \frac{1}{m}\sum_{i}Z^{(i)} $$

$$ \sigma^2 = \frac{1}{m}\sum_{i}(Z^{(i)}-\mu)^2 $$

$$ Z_{\text{norm}}^{(i)} = \frac{Z^{(i)} - \mu}{\sqrt{\sigma^2 + \epsilon}} $$

$$ \tilde{Z^{(i)}} = \gamma Z_{\text{norm}}^{(i)} + \beta $$

일반적인 normalization을 $$Z_{\text{norm}}$$으로 표현했습니다. $$\tilde{Z^{(i)}}$$는 normalization의 결과가 평균 0에 분산 1로 나오기 때문에 그것을 조정한다고 생각하시면 되겠습니다.

## Fitting Batch Norm into a neural network

Batch Normalization을 neural network에 적용할 때 고려해야 할 사항에 대해 알아보겠습니다. neural network의 데이타 흐름은 다음과 같습니다.

$$X \xrightarrow{W^{[1]}, b^{[1]}} Z^{[1]} \to a^{[1]} \xrightarrow{W^{[2]}, b^{[2]}} Z^{[2]} \to ... $$

여기에, activate function의 입력을 normalization을 적용하면,

$$X \xrightarrow{W^{[1]}, b^{[1]}} Z^{[1]} \color{maroon}{\xrightarrow[\text{Batch Norm}]{\beta^{[1]}, \gamma^{[1]}} \tilde{Z^{[1]}} } \to a^{[1]} \xrightarrow{W^{[2]}, b^{[2]}} Z^{[2]} \to ... $$

과 같은 모습이 됩니다. 새로운 parameter가 더욱 추가된 셈인데, $$W, b, \gamma, \beta$$까지 구해야 합니다. 한 가지 기억하셔야 할 점은, mini-batch를 사용하는 경우에는 $$\gamma, \beta$$는 해당 epoch에서 산출된 값을 사용해야 한다는 점 입니다. $$W, b$$는 epoch마다 이전 값을 바탕으로 계산되는 반면에, $$\gamma, \beta$$는 epoch마다 새로 구해져야 합니다. 또 다른 점으로는 $$b$$를 구할 필요가 없다는 점입니다. $$b$$는 $$Z = W \dot A + b$$였습니다만, Batch Norm으로 평균값을 제외시키게 되면, 자연스레 $$b$$값도 사라지기 때문입니다. 그 역활은 $$\beta$$에 의해 진행되는 셈입니다. $$b, \gamma, \beta$$의 모두 $$(n^({[l]}), 1)$$의 shape을 가지게 됩니다.

parameter가 더 늘어났다는 것은, backward propagation 과정에서 $$W^{[l]}, \gamma^{[l]}, \beta^{[l]}$$을 모두 계산해주어야 합니다.

## Why does Batch Norm work?

Batch Norm을 통하면, activation function의 입력이 적절한 범위내에서 표현되므로, gradient descent를 계산 할 때 더욱 빠르게 작동할 수 있습니다.
또한, neural network가 가지는 covariate shift문제를 감소시키는 역활을 합니다. covariate shift는 기존의 $$X$$와 조금이라도 다른 분포를 가진 $$X$$를 만나면 다시 training을 거쳐야 하는 문제를 뜻합니다. neural network는 각 layer간이 긴밀하게 서로 영향을 미치게 되는데, 다른 분포의 입력에 대해서 전체 neuron이 반응하기 때문입니다. Batch Norm을 사용하면, 입력값의 범위가 제한적으로 바뀌면서, 변화량도 줄어들게 됩니다. 즉, neuron이 상대적으로 독립적으로 변하게 되는 셈입니다. 
Batch Norm을 통하면, 어느 정도 regularization이 일어나는 것으로 보고되었습니다. epoch의 평균과 분산을 반영하면서, data의 noise를 줄이는 역활을 한다고 합니다. 그렇다고, regularization으로 사용하는건 좋지 않다고 합니다.

## Batch Norm at test time

Batch Norm은 mini-batch 마다, $$\mu, \sigma^2$$를 구하게 되는데요, 정작 test시에 한번에 하나씩 수행하게 되므로, $$\mu, \sigma^2$$를 구할 수 없게 됩니다. 이런 경우에는, mini-batch시에 구했던 값들을 이용해서, exponentially weighted average를 이용해거나 다른 방법을 통해서 $$\mu, \sigma^2$$를 산출해야 합니다. 그 외에 $$W, \gamma, \beta$$는 training에서 구한 값을 사용합니다.


# Multi-class classification

## Softmax Regression

지금까지는 output layer에 sigmoid함수를 이용해서 $$\{y \in {0, 1}\}$$인 경우만 살펴 보았습니다. 만약, 고양이, 강아지 그리고 병아리를 식별하는 neural network를 만든다고 하면, 어떻게 해야 할까요? neural network 표현해야 할 값의 종류를 $$C$$(class)를 이용해서 표기하는데, 3가지 경우가 있으므로, $$C = 3$$이라고 표기합니다. 그리고 output layer에서 sigmoid로는 당연히 어렵겠죠. SoftMax라는 함수를 사용하게 됩니다. 먼저, output layer의 $$Z$$를 구합니다.

$$ Z^{[L]} = W^{[L]}a^{[L-1]} + b^{[L]} $$

activation function인 softMax를 적용합니다. 

$$ t = e^{Z^{[L]}} $$

$$ a^{[L]} = \frac{e^{Z^{[L]}}}{\sum t} $$

$$a^{[L]}$$은 $$( C, 1)$$의 크기를 가지며, 각 class마다의 확률을 가집니다. 결과적으로 총합은 1이 되는데, 각 확률은 위에서 보는 것처럼, $$e$$의 지수승에 t벡터의 합으로 나눈 값이 됩니다.

## Training a softmax classifier

"soft max"라는 이름은, "hard max"에서 왔습니다. "hard max"는 0과 1로 이루어진 벡터를 뜻합니다. soft max는 사실, logistic regression을 C개의 class를 표현하는 형태로 일반화했을 뿐입니다. 하지만, Loss function은 조금 다릅니다.

$$ \mathscr{L}(\hat{y}, y) = -\sum_{j=1}^{4}y_{j}\log\hat{y}_{j}$$

Loss function을 정의했으므로, back-propagation에 사용할 미분식도 구해보면,

$$ \frac{\partial \mathscr{L}}{\partial Z^{[L]}} = - \sum_{j} y_{j}\frac{\partial \log \hat{y}}{\partial Z^{[L]}}  $$

$$ dZ^{[L]} = \hat{y} - y $$

어떻게 저 값이 나오는지는, [The Softmax function and its derivative](http://eli.thegreenplace.net/2016/the-softmax-function-and-its-derivative/)를 참고하시면 되겠습니다.

# Introduction to programming frameworks

## Deep learning frameworks

우리가 다루는 수준은 numpy만으로도 충분하지만, 조금 더 복잡해진다면 frameworks를 고려할 필요가 있습니다. 선형 대수 수준이 아니라, 추상화된 고차원의 API를 이용하는 것이 훨씬 효율적이기 때문입니다. 워낙 많은 frameworks가 있으므로, 개개에 대한 설명 보다는 선택시 고려사항에 대해 이야기 해보겠습니다.

- 프로그래밍이 쉬울 것 
- 빠른 성능
- 완전히 open source 일 것

정도가 되겠네요.

## TensorFlow

간단한 튜토리얼을 진행합니다. 시중에 다루고 있는 책들이 많으므로, 부연 설명은 하지 않겠습니다.

