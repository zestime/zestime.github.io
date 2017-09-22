---
layout: post
title:  "ML Strategy(1)"
date:   2017-09-21 10:08:45 +0900
categories: classes ml-strategy
---

> ### 시작에 앞 서...
> 이 글은 Coursera의 [Structuring Machine Learning Projects](https://www.coursera.org/learn/machine-lerning-projects) 수업을 정리한 자료입니다.<br/>
> 교수님의 말씀을 잘 이해하지 못한 부분도 있으니, 수업을 들으시면 더욱 좋을 것 같습니다.
>
> - Week1 : [ML Strategy(1)]({{ site.baseurl }}{% link _posts/2017-09-21-ml-strategy-1.md %})
> - Week2 : [ML Strategy(2)]({{ site.baseurl }}{% link _posts/2017-09-22-ml-strategy-2.md %})
{:.intro}

이번 강의는 machine learning system을 구현하면서 취해야할 접근법에 대해서 소개하고 있습니다. 어떻게 진단하고, 효과적인 부분을 우선적으로 해결할지에 대해 다루고 있습니다. 

# Introduction to ML Strategy

## Why ML Strategy

정확도를 높이는 방법에는 여러가지가 있을 수 있습니다. 예를 들어, 더 많은 데이타를 통해 정확도를 높이는 방법이 있을 수 있습니다. 데이타를 추가적으로 수집하는데 6개월 가량이 걸리는데, 수집된 데이타를 통한 정확도의 향상이 미비하다면 좋은 선택이 아니었다고 볼 수 있습니다. Cost effective한 최선의 결정을 내리기 위해서, 전략이 필요할 수 밖에 없습니다.

## Orthogonalization

직교화한다는 뜻의 Orthogonalization은 neural network를 튜닝하면서 직면하게 되는, parameter간의 복잡성을 최대한 직교성을 가지도록 다루는 방법입니다. 서로 복잡하게 얽혀있으면, 적절한 값을 찾기가 더욱 어렵기 때문에, 각 parameter의 적용 범위를 독립적으로 구성하는 것이 효율적인 접근이 되겠습니다. 이는 각 parameter가 전체 성능 및 각 단계에 어떠한 영향을 미치는지 알고 있어야함을 뜻합니다. 모델에 맞게 각각의 parameter를 적용되는 범위에 따라 직교화하려는 노력이 필요합니다.

# Setting up your goal

## Single number evaluation metric

Logistic Regression과 같이, $${0, 1}$$로 끝나는 경우에는 알고리즘간의 비교가 상대적으로 쉽습니다. 우리가 다루는 문제는 대부분 Multi-classfier이기 떄문에, 오차를 특정하기가 쉽지 않습니다. 그렇다고 할지라도, 평균 오차와 같은 하나의 수치로 대변할 수 있는 척도를 만들어야 합니다. 알고리즘간 비교가 가능해야 한다는 것이죠.

## Satisficing and Optimizing metric

정확도외에도 실행시간(running time)도 고려해야할 사항이 될 수 있습니다. 사실, 정확도 외에도 여러가지의 제한 사항들이 있을 수 있습니다. 이러한 제한 사항 중에서도 만족해야 하는 경우(satisficing)와 최적화를 해야 하는 경우(optimizing)으로 나눠지게 됩니다. 이에 따라, metric 또한 설정되어야 합니다. 

## Train/dev/test distributions

dev set과 test set을 선정할 때에는, train set과 같은 범주에서 선택해야 합니다. 결과적으로는 우리가 예측하려는 데이타에서 시작되어야 합니다. 가령, 특정한 group을 dev set으로 사용하는 경우, train set은 물론 예측에서도 낮은 정확도를 보이게 됩니다. 왜냐하면, training한 목표와 dev set의 목표가 다르기 때문입니다. 따라서, dev set을 선정할 때에는 최종적인 예측하려는 데이타에 근사해야 하며, train set도 이를 포함하고 있어야 합니다.

## Size of the dev and test sets

예전에는 데이타를 7:3으로 train : test로 나누거나, 6:2:2으로 train:dev:test으로 나누었습니다. 현재는 과거보다 훨씬 많은 데이타를 다루게 되었으며, 98:1:1의 비율로 나눌 수 있습니다. 즉, dev와 test에 많은 데이타를 사용하지 않지만, 데이타 자체가 양적으로 증가되어서 충분히 많은 수를 확보할 수 있습니다. 

## When to change dev/test sets and metrics

알고리즘간의 비교를 위해서 설정했던 evaluation metric은 어디까지나 우리가 지향하는 목표를 위해서 필요한 척도 입니다. 만약, 그 목표를 제대로 반영하지 못한다는 것을 알게 되었다면, 주저 없이 변경되어야 합니다. 물론, 찾아낸 문제점을 포함하는 형태로 개량되어야겠죠. 또한 data 역시 마찬가지입니다. dev set이 우리의 목표에서 동떨어져 있음을 발견하게 되면, 전체적인 data의 수정이 필요할 수 밖에 없습니다. 예측하려는 데이타를 최대한 반영할 수 있도록 조정이 필요합니다.

# Comparing to human-level performance

## Why human-level performance?

neural network를 통해서 예측된 값은 사람의 수준(human-level)을 훨씬 뛰어 넘을 수 있습니다. 하지만, 그렇다고 모든 상황에서 정확한 답을 낼 수는 없습니다. 이를 Bayes Optimal Error(베이즈 에러)라고 하는데, 우리가 training할 수 있는 한계라고 생각하시면 되겠습니다. training을 진행하다보면, 성능이 사람의 수준 이전에는 학습이 빠른 속도로 이루어 집니다. 사람의 수준을 넘어서고는 학습이 더디게 진행되다가, 결국 Bayes error에 수렴하게 됩니다. 여기에는 여러가지 이유가 있는데, data 자체가 사람에 의해 만들어진 탓도 있으며, 사람의 수준이 상당히 높기 때문에 최적화할 여지가 없는 탓이기도 합니다. 또한 bias/variance와 같은 도구들이 큰 의미를 가지지 못하기 때문입니다.

## Avoidable bias

사람의 수준(human-level)이 Bayer error와 비슷한 경우가 있습니다. 대표적인 경우가 Conputer Vision인데요, 이런 경우에는 Human-level이 곧 Bayer error로 볼 수 있기 때문에, 정확도가 human-level을 달성하면 그 이상으로 개선하기가 무척 어렵습니다. 다른 부분을 살펴보아야 하는데, 가령 dev set과 train set간에 차이가 있다면 variance로 보고 이 부분의 개선을 진행하는 편이 더 효과적입니다.

## Understanding human-level performance

사람의 수준(human-level)을 어떻게 설정할 수 있을까? 어떻게 설정한다고 해도, 사람의 수준은 Bayes error보다는 낮을 수 밖에 없습니다. 이렇게 사람의 수준을 근거로 한 Bayes error를 proxy for Bayes error 혹은 estimated Bayes error라고 합니다. 하지만, Bayes error를 판가름 할 수 있는 척도가 바로, human-level로 볼 수 있겠습니다. 데이타에 따라 다르지만, train error와 dev error를 두고 Bias reduction과 variance reduction을 택할 때, human-level에 근거한 estimated Bayes error를 사용합니다. 

## Surpassing human-level performance

사람의 수준을 뛰어넘은 Machine Learning System은 현재에도 많습니다. 충분한 데이타와 잘 구성된 시스템만 있다면, 충분히 사람의 수준을 뛰어 넘을 수 있습니다.

## Improving your model performance

이번에는 Avoidable bias와 variance가 의심될 때 취할 수 있는 방법을 정리해 보겠습니다.

* Avoidable bias : Human-level과 Training error와의 차이가 큰 경우
  * Train bigger model
  * train longer/better optimization algorithms
  * NN architecure/hyperparameters search
* Variance : Train error와 dev error의 차이가 큰 경우
  * More data
  * Regularization
  * NN architecure/hyperparameters search

