---
layout: post
title:  "2. ML Strategy(2)"
date:   2017-09-22 10:08:45 +0900
categories: classes ml-strategy
---

> ### 시작에 앞 서...
> 이 글은 Coursera의 [Structuring Machine Learning Projects](https://www.coursera.org/learn/machine-lerning-projects) 수업을 정리한 자료입니다.<br/>
> 교수님의 말씀을 잘 이해하지 못한 부분도 있으니, 수업을 들으시면 더욱 좋을 것 같습니다.
>
> - Week1 : [ML Strategy(1)]({{ site.baseurl }}{% link _posts/2017-09-21-ml-strategy-1.md %})
> - Week2 : [ML Strategy(2)]({{ site.baseurl }}{% link _posts/2017-09-22-ml-strategy-2.md %})
{:.intro}

# Error Analysis

## Carrying out error anlysis

성능을 높이기 위해서 여러가지 노력을 할 수 있습니다만, 무엇에 투자할지 결정하는 것이 선행되어야 합니다. 이를 위해선, error anlysis를 진행해야 합니다. dev set에 일부러 error를 만들어서, error의 분포를 측정하는 것 입니다. error의 분포는 우리가 향샹할 수 있는 성능을 예측하는 단초가 될 수 있습니다. dev set과 test set만을 통해서는, 우리의 알고리즘이 어디에 약점이 있는지 확연히 드러나지 않습니다. 그래서 error의 양을 늘려 어떤 입력에 대해서 error가 많이 발생되는지를 파악할 수 있습니다.

## Cleaning up incorrectly labeled data

Supervised Learning에서 각각의 입력에 대한 결과값인 label은 중요합니다. 하지만, label이 무조건 맞다고 가정하기는 어렵습니다. 그 중에 오기된 label도 존재할 수 있습니다. 하지만, deep learning algorithm은 이러한 random error에는 상당한 내성을 가지고 있습니다. 물론, 정상적인 input 데이타가 충분히 많고, 잘못된 데이타가 적다고 한다면, 잘 작동합니다. 다만, systematic error와 같이 random하지 않고 어떤 특성에 대해서 모두 오기되었다면, 이는 문제가 될 수 있습니다.
이런 오기에 대해서도, 앞서 진행한 error analysis를 진행합니다. 모든 errord세 대해서, 잘못된 표기로 인해 발생된 것인지 측정할 필요가 있습니다. 측정을 통해, 잘못된 표기를 바로 잡았을 때 향상될 성능을 예측할 수 있기 떄문입니다. 

## Build your first system quickly, then iterate

새롭게 machine learning system을 만든다면, 다음의 과정을 통해서 진행하는 것이 좋습니다.

1. dev/test set 설정 : ML의 target을 명확히 합니다.
1. 시스템 구현 : 빠르게 구현해 봅니다.
1. 중요한 다음 단계 선정 : Bias/Variance 혹은 Error 분석으로 효과적인 다음 단계를 도출합니다.

# Mismatched training and dev/test data

## Training and testing on different distributions

아시다시피, dev/test set은 예측하고자 하는 데이타를 가장 잘 표현하고 있어야 합니다. 그에 맞게 train set 또한 같은 distribution을 가져야 합니다. 하지만, 현실적으로 많은 데이타를 구하기 어렵기 때문에, 서로 다른 distribution을 혼용하여 사용해야 하는 경우가 발생됩니다. 그럴 때에도, dev/test set은 목적에 맞게 설정되어야 하며 train set은 혼용하여 사용하도록 합니다. 이렇게 다른 distribution을 다루는 기술은 나중에 부연토록 하겠습니다.

## Bias and Variance with mismatched data distributions

서로 다른 distribution을 사용할 때는, train-dev 라는 또 다른 그룹을 설정해야 합니다. train set에 포함된 새로운 distribution의 data를 train-dev set으로 부릅니다. error를 진단할 때, training error, train-dev error, dev error를 구하여 비교하게 됩니다. train-dev error가 추가되었는데, 이는 variance를 진단하는 척도가 됩니다. 즉, train error와 train-dev error가 많이 차이가 난다면 variacne가 있다고 볼 수 있습니다. 그런데, 만약 train-dev error와 dev error가 많이 차이날 수 있을까요? 둘은 같은 distribution이지만 데이타가 많지 않았기 때문에 충분히 발생될 수 있는 상황입니다. 이를 data mismatch라고 합니다.

## Addressing data mismatch

train-dev set과 dev set을 분석하여, 무엇때문에 이러한 mismatch가 생기는지 알아볼 필요가 있습니다. error의 특성을 밝혀, 그러한 데이타를 더욱 train시켜야 합니다. mismatch한 데이타와 비슷한 데이라를 모을 수가 없다면, 합성(synthesis)을 이용하여 만들어야 합니다. 예를 들어, 차량 소음으로 인해 음성 인식의 성능이 떨어진다면, 차량 소음과 정상적인 음성 데이타를 합성하여 train 데이타를 만들어야 합니다.

# Learning from multiple tasks

## Transfer learning

이미 잘 훈련된 neural network가 있다면, 그것을 사용해 비슷한 새로운 network를 만들 수 있습니다. 이를 transfer learning이라고 합니다. transfter learning을 하기 위해서는, input은 같아야 하고, 선행 학습된 데이타가 훨씬 많은 경우에 효과적입니다. 적용하는 방법은 이미 학습된 신경망의 마지막 layer를 새로운 작업에 맞게끔 변화시키면 됩니다. 이 때, 이미 구해진 $$W, b$$값은 계속 사용하되, backpropagation step에서 수정할지는 데이타에 따라 결정하면 됩니다. 주로, 잘 훈련된 신경망이 있는 상태에서, 데이타 적지만 비슷한 류의 입력을 가지는 새로운 신경망을 만들때 적절한 접근이 될 수 있습니다.

## Multi-task learning

transfer learning이 기존의 machine learning에 새로운 layer를 추가해서 처리하는 방법인 반면, 동시에 여러 가지 일을 한 번에 진행하는 형태를 multi-task learning이라고 합니다. 예를 들어, 물체를 인식하는데, 자동차를 인식하는 task와 신호등을 인식하는 task를 동시에 진행하는 방법을 뜻합니다. 장점으로는 자연스럽게 데이타가 늘어난다는 점입니다. n개의 task를 동시에 수행하는데, 각 task마다 1000개의 데이타가 있다고 하면, $$m$$은 $$n * 1000$$으로 늘어납니다. 이를 통해서, 각각 훈련시키는 것 보다, 좋은 결과를 가져올 수 있습니다. softmax와 비슷한 것 같다는 생각이 들 수도 있는데, $$\hat{y}$$는 $$\begin{bmatrix} 1 \\ 0 \\ 1 \end{bmatrix}$$과 같이 합이 1이 넘을 수 있기에, softmax와는 다른 결과를 가지게 됩니다. 
실제적으로는 transfer learning보다는 잘 사용되지 않는다고 합니다.

# End-to-end deep learning

## What is end-to-end deep learning

machine learning 작업을 진행하다 보면, 하나의 작업은 부분으로 나누어 pipeline을 구성해서 처리하기도 하고, 혹은 처음부터 끝까지 하나의 작업(end-to-end)으로 구성하기도 합니다. 이러한 작업 구분은 전적으로 가용한 데이타에 따라 달라집니다. end-to-end로 구현해서 적용할 데이타가 많다면 충분히 좋은 성능을 보이게 되고, 반대로 나누었을 때 적용할 수 있는 데이타가 많다면 서로 다른 신경망으로 구성하는게 더 좋은 성능을 보일 수 있습니다.

## Whether to use end-to-end deep learning

end-to-end 접근은 다음의 장점이 있습니다.

- 데이타 자체를 반영 : 인간의 인식으로 인한 왜곡이나 변환이 없어서 본연을 잘 표현
- 데이타 개입할 일이 적습니다.

단점은 다음과 같습니다.

- 다량의 데이타가 필요
- 적극적인 데이타 참여가 불가능합니다. 

end-to-end 접근을 하기 위해선, $$X \to Y$$의 복잡성을 배울 수 있을 정도의 충분한 데이타가 있어야 합니다.
