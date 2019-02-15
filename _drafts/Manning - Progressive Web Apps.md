---
layout: post
title:  "Progressive Web Apps"
date:   2018-10-01 10:42:45 +0900
categories: books
---

*나는 아무것도 알 수 없었다. 문을 열기 전까지는 말이다.*

#### Summary

* 2018/09 ~ 10
* PWA(Progressive Web Apps)에 대한 이해를 높이는 책, PWA에 대한 concept을 잡고 유용한 tips들을 학습할 수 있음



## 8. Building more resilient applications in Progressive Web Apps

이번 장에서는 SPOF(Single point of Failure)에 대해 다룬다. 특정, request가 지나치게 오래걸리는 lie-fi(멍텅구리 와이파이, 신호강도는 높지만 download는 받을 수 없는)와 같은 경우에서 페이지 전체가 지연되는 것을 줄이는 방법을 소개하고 있다. 이런 류의 작업은 익숙한 패턴이겠지만, ```Promise.race```에서 timeout과 실제 fetch를 경쟁시키는 방식으로 이뤄진다. 여기서 조금 다른 점은 이를 진행하는 주체가 serviceWorker라는 점이다. 특정, 3rd party library나 font가 장애가 생겨도, 그런대로 볼 수 있는 페이지를 제공하는 것이 그 목적이다. 이는 webfont와 같이 look and feel에 관련된 내용이라면 간단하지만, CSS와 같이 layout을 담고 있는 파일의 경우에는 제대로 보여줄 수가 없게 된다. 이러한 문제 때문이라도, 앞서 살펴본 cache에 look and feel과 관련된 파일들을 담고 있는 것이 바람직하다.


## 9. Keeping your data synchronized

일반적으로 API에 대한 request가 발생되면, 즉각적으로 server에 연결합니다. 하지만, 일시적인 offline과 같은 일시적으로 연결을 할 수 없는 network 상태라면 차후에 다시 시도해야 합니다. 몇몇 우수한 web app의 경우에는, 자동으로 offline에서 online으로 바뀌는 것을 감지하고 실패한 요청을 다시 보내는 경우가 있습니다. 이러한 과정을 sync라고 부르며, **SyncManager**가 지원되는 환경에서만 사용할 수 있습니다. 당연한 이야기지만, 실패한 요청들을 따로 관리할 필요가 있습니다. 그래야, 차후에 online상태에서 다시 요청을 보낼 수 있기 때문입니다. 그러한 매개체로 indexDB를 사용하고 있습니다, 이는 main thread와 service worker간의 데이타를 공유하는 방법으로 사용되었다고 보시면 되겠습니다. 이러한 구조가 싫다면, message queue를 service worker에서 구현해도 될 것 같습니다. 그 외에는 **PeriodidcSync**에 대한 언급이 잠시 있었습니다. 일정 간격으로 server로부터 새로운 데이타를 받아오기에 좋은 기능입니다. 예전에는 interval로 처리하던 것과 비슷하지만 조금 더 상세한 options들을 가지고 있습니다. 특히나, battery와 cellural network에 대한 설정을 제공합니다.


## 10. Streaming Data

stream은 그 자체로서 상당히 강력한 기능이다. 어떠한 file이 있다고 할 떄, file의 전체가 전송되기 전까지는 우리는 아무런 작업을 할 수가 없다. 그러나, stream이라면 애기가 좀 달라진다. stream은 일종의 데이타가 지나가는 통로에 가깝다. stream을 통해 전달되는 data를 chunk라 하는데, 이 chunk의 합이 file이 될 수도 있고 혹은 비동기적인 전송이 될 수도 있다. 이번 챕터에서는 serviceWorker가 fetch의 결과를 stream으로 바꿔서 전달하는 방법에 대해 다루고 있다. 

