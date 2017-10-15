---
layout: post
title:  "1. Recommender System?"
date:   2017-10-12 10:08:45 +0900
categories: recommender-system
---


> ### 시작에 앞 서...
> 아시는 분들도 계시겠지만, 제가 요즘 Recommender System(추천시스템, 이하 RS)에 관심이 많습니다. 검색이 색인에 대한 결과라고 한다면, 추천은 색인에 대한 점수가 유저마다 달라야 해결할 수 있는 문제입니다. 이러한 RS에 대해서 좀 다뤄볼까 합니다.
>
> - [1. Recommender System?]({{ site.baseurl }}{% link _posts/2017-10-12-recommender-system.md %})
> - [2. Recommender System with Deep Learning]()
> - [3. Restricted Boltzmann Machine for Collaborative Filtering]()
> - [4. Collaborative Deep Learning for Recommender Systems]()
{:.intro}

Deep Learning(딥러닝, 이하 DL)이 유성우 같다면, RS는 북극성같은 존재입니다. Machine Learning의 여러 예시 중에서, 가장 실질적이고 효과적인 시스템이 바로 RS입니다. DL의 부상으로 RS에도 많은 연구가 이뤄지고 있습니다. 
첫 번째 글에서는 RS에 대한 가장 기본적인 사항이자, 익히 알려진 Collaborative filtering을 다뤄 보겠습니다. 이후, [Deep Learning based Recommender System: A Survey and New Perspectives]()이라는 논문을 소개하면서 RS에 DL을 적용하려는 현황을 다룰까 합니다. 현황을 본 다음에는, 인용이 많은 논문 2편을 살펴볼 생각입니다.
 
## Recommender System?

RS는 유저에 좋아할 것 같은 제품을 추천해주는 시스템을 말합니다. 옛날에는 같은 Category의 제품을 보여주거나, MD와 같은 인력이 직접 선택한 항목을 보여주도록 하였지만 지금은 사람의 감이 아닌 계산을 통해 더욱 정확한(고객의 기호에 맞는) 제품을 보여줍니다. 즉, 객단가를 높이기 위한 방안으로 RS에 대한 투자가 이뤄지고 있는데요, youtube의 경우에는 60%의 click이 바로 추천 리스트에서 이뤄진다고 합니다. 또 다른 예로는, 영상 서비스를 제공하는 Netflix는 80%정도가 이뤄지고 Amazon의 경우에는 40%에 가까운 구매가 추천을 통해 발생됩니다. 조금 더 부연하자면, Netflix의 경우에는 최신 영화보다는 조금 오래되었지만 고객의 기호와 잘 맞는 제품이 이익에 도움이 되며, Amazon의 경우에는 최신 상품이든 스테디 셀러든 판매 가능성이 높은 상품을 제공하는 것이 중요하다고 합니다.


## RS의 분류

### Collaborative Filtering 

Collaborative Filtering를 들어보셨을 것 같습니다. 직역하자면 '공동의 필터링'이 되겠습니다. 감이 전혀 오지 않으시죠? 저도 그랬습니다. Collaborative는 각 유저들의 관심사들을 한 데 모아 그 자체를 하나의 필터로 사용하는 것을 뜻합니다. 여기에는 하나의 전제가 있는데, 과거에 같은 경험을 가진 유저는 미래에도 같은 기호를 가질 것으로 간주하게 됩니다. 즉, A와 B가 과거에 같은 영화를 보았다면, 미래에도 B는 A가 본 영화를 볼 확률이 높다고 접근하는 것이죠. 이렇게 유저의 관점에서 진행하는 것을User-based Collaborative filtering이라 합니다. 반면에, 과거에 본 영화들을 바탕으로 그와 가장 비슷한 영화를 추천하는 것을 Item-based Collaborative filtering이 됩니다.

- **User-based Collaborative filtering** : 유저에 포커스를 맞추므로, 유저간의 유사도를 측정하게 됩니다. 그리고 가장 유사한 유저에게 더욱 가중치를 두고, 유저가 봤을 때의 평점을 계산하게 됩니다.

- **Item-based Collaborative filtering** : 제품간의 유사도를 측정합니다. 유저의 기호에 근거해서 가장 근접한 제품을 찾아내게 됩니다. 

### Content-based recommender system

Collaborative filtering이 유저 혹은 제품이라는 하나의 편향되었다면, Content-based recommender system은 user-profile과 item-profile을 만들어 그 간극을 매꾸는 역활을 합니다. 유저의 관심사에 각 제품의 특징까지 같이 고려해서 추천하는 시스템이라고 할 수 있습니다. 예를 들어, user-profile을 평점으로 생각한다면, item-profile은 item의 자체의 특성으로 genre, director와 같은 부가적인 속성이라고 볼 수 있습니다. 

### Hybrid recommender system

Collaborative filtering은 그 자체로도 좋은 RS임에는 틀림없지만, 몇 가지 문제가 있습니다. 유저의 평점에 의존해기 때문에, 평점이 충분히 갖춰지지 못한 초기 유저의 경우에는 틀리기 쉽습니다. 그렇기 때문에, 초기에는 Content-based의 결과치를 높게 쓰고, 어느 정도 유저의 평점이 갖춰지면 그 때부터는 USer-based Collaborative filtering에 더욱 가중치를 주는 형태로 사용할 수 도 있습니다.

### Context-aware recommender system

유저의 관심은 언제나 같을 수 있을까요? 유저의 상황에 맞게, user-profile을 달리하는 것을 Context-aware라고 합니다. Context를 결정짓는 요소는 서비스에 따라 다양할 수 있습니다. 장소라던지, 날씨, 날짜, 요일 같은 정보에 따라 다른 추천 결과를 제공할 수 있습니다. 
제가 가장 궁금해 하던 RS이며, 검색이 1차원 적인 결과라 한다면 RS는 유저에 따른 2차원에 해당되며, Context-aware는 유저를 또 다시 나누어 생각하게 되므로 3차원이라 볼 수 있습니다.


## Collaborative filtering 예시

VOD 서비스를 제공하는 사이트가 있습니다. 이 사이트에 모든 유저는 영화를 보고 꼭 평점을 남깁니다.(그럴리가 없다는게, 현실 세계에서의 문제이기도 합니다.) 과거에 비슷한 영화를 본 A와 B가 있습니다. 둘 다 비슷한 영화를 시청했고, 평점도 비슷한 분포를 가집니다. 그런 상태에서 A가 새로운 영화 '라라랜드'를 시청하고는 평점으로 만점을 주었고, '컬트 오브 처키'에는 3.5점만 주었습니다. B는 아직 '라라랜드'를 보진 않았지만, 아마도 만점에 가깝울 것이라고 보는게 *User-based Collaborative filtering*입니다.

그런데, A와 B는 과거에 비슷한 영화를 시청했습니다만, B는 공포 영화에는 항상 평점을 좋게 주었습니다. 사실상, 공포 영화 매니아 였다고 볼 수 있겠습니다. '라라랜드'가 평점은 높으나, '컬트 오브 처키'가 공포 영화이기에 가중치를 주어서 이를 추천할 수도 있습니다. 이러한 경우는, *Content-based Recommender System*가 되겠습니다. 공포 영화라는 것은, 평점이나 사용자들의 시청과는 상관없으며 제품 자체의 특성이므로, 제품에 대한 부가적인 정보가 필요하다는 뜻이 됩니다. 

그렇지만, B는 공포 영화를 주로 밤에 보는데, 일요일은 절대 보지 않는다고 가정해 보겠습니다. 이런 상황에서는, B에게 영화를 추천하는데 '컬트 오브 처키'를 보여주는 것은 요일에 따라 다른 문제가 됩니다. 즉, 일요일에는 공포에 대한 가중치가 낮게 책정되어야 한다는 것이죠. 사용자의 특정 상황(Context)에 대해 고려하기 때문에, *Context-aware Recommender System*이 되겠습니다. 요일을 예로 들었습니다만, 모바일 기기인지, TV인지에 따라 나눌 수도 있으며, 다양한 차원을 설정할 수 있겠습니다.

책상 위에서는 너무도 단순하리만큼 깔끔한 접근이지만, 현실은 상당한 거리가 있을 수 있습니다. Context-aware의 경우에는 더 많은 정보를 필요로 하지만, 우리는 평점을 남기는 것을 너무도 싫어합니다. 목적이 해소된 상태에서는 시스템에 무언가를 더 해줄 필요가 없는 것이죠. 

## 문제점...

충분한 시간이 있고, 사용자들이 자신들의 정보를 남기는 것에 협조적이라고 한다면 우리는 개개인에 정확하게 맞는 추천을 할 수 있습니다. 하지만, 시간도 없고, 유저의 피드백도 없습니다. 이런 초기 상태를 **cold start**라고 합니다. 데이타가 어느 정도 갖춰지기 전까지는 형편없는 성능을 보여주게 됩니다. 또한, review와 같은 경우, 대부분 사용자들이 제대로 된 값을 남기지 않기 때문에, Vectorize를 하면 sparse(대부분의 값이 0이 되는)한 형태를 가진다는 점입니다. Space Vector는 0벡터에 가깝기 때문에, 곱해지는 User에 대한 벡터에 상관없이 0에 가까운 값을 가질 수 있습니다. 이러한 문제점을 해결하고자 다른 시도가 있었으니, 그것은 다음 포스팅에서 다루도록 하겠습니다.


## Reference

- [Building Recommendation Engines, 2016, PACKT](https://www.packtpub.com/big-data-and-business-intelligence/building-recommendation-engines)
