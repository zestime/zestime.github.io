---
layout: post
title:  "2. Recommender System with Deep Learning"
date:   2017-10-13 10:00:00 +0900
categories: recommender-system
---


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
