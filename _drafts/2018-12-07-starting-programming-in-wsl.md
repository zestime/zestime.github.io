---
layout: post
title:  "Starting programming in WSL"
date:   2018-12-06 10:00:00 +0900
categories: WSL
---

## WSL에서 개발하기

WSL(Windows Subsystem for Linux)은 Windows 10부터 제공하는 window 내의 linux 시스템이다. 사실, 가상 머신으로 작동되기 때문에 linux와 거의 동일한 환경을 사용할 수 있게 된다. 이러한 시도는 이전에는 docker나 vagrant와 같은 도구를 사용해서 접근되었으나, Microsoft 역시 이러한 변화를 감지하고 자사의 OS에서 제공키로 한 것이다. 2년 전에 WSL은 불안한 점들이 제법있었으나, 정식 서비스를 하고 관련된 tool 역시 갖춰지면서, 개발자에겐  Window의 필수 요소로 자리잡고 있다. 오늘은 이러한 WSL에서 개발 환경을 다루는 방법을 조금 더 살펴 볼까 한다.

## Install

Vanila 상태의 Linux는 사실 처참하다. 기본적인 도구가 많지만, 그 이상의 좋은 도구는 더욱 많기 때문이다. 이러한 편차를 줄이기 위해서, 아래의 도구들을 설치할 필요가 있다.


> ### 시작에 앞 서...
> 아시는 분들도 계시겠지만, 제가 요즘 Recommender System(추천시스템, 이하 RS)에 관심이 많습니다. 검색이 색인에 대한 결과라고 한다면, 추천은 색인에 대한 점수가 유저마다 달라야 해결할 수 있는 문제입니다. 이러한 RS에 대해서 좀 다뤄볼까 합니다.
>
> - 1. Recommender System?
> - 2. Recommender System with Deep Learning
> - 3. Restricted Boltzmann Machine for Collaborative Filtering
{:.intro}

Deep Learning은 거의 대부분 분야에 이미 적용되고 있습니다. 아직, 적용되지 않았다면 그것은 적용할 수가 없거나 너무 어렵기 때문일 것입니다. 그에 비하면, Recommender System(이하, RS)은 이미 예전부터 신경망을 적용하려는 노력이 있었습니다. Hitton 교수님의 [Restricted Boltzmann Machine for Collaborative Filtering]() 논문에서 알 수 있듯이, 2007년에 이미 시도되고 있었습니다. 그렇다면, 현재는 어떠할까요? 그 답을 찾고자, 이번 포스트는 [Deep Learning based Recommender System: A Survey and New Perspectives]
 


## RS의 결과에 따른 분류

그럼 RS는 각 제품마다 점수를 매기는 건가요? 그럴수도 아닐 수도 있습니다. 결과값으로 분류를 하자면 다음과 같습니다. 

- Rating Prediction : 기존 평점을 근거로 아직 유저가 보지 못한 제품들의 평점을 예측합니다. 비슷한 유저가 있다면 더욱 정확한 근거를 마련할 수 있겠죠. 다만, 평점이 어느 정도 갖춰줘야 제대로 작동한다는 단점이 있습니다.

- Ranking Prediction(Top-n) : 각 제품에 대해서 점수를 갖추고, 그에 따라 유저에게 n개를 추천합니다.

- Classification : 세부적이고 유저에 가까운 카테고리에 제품을 분류하여 유제에게 추천합니다.



## Reference

- [Building Recommendation Engines, 2016, PACKT](https://www.packtpub.com/big-data-and-business-intelligence/building-recommendation-engines)
