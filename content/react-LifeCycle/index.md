---
emoji: ✍️
title: React Life-Cycle methode in class
date: '2021-11-13 15:00:00'
author: me
tags: React Life-Cycle
categories: React
---

React는 LifeCycle을 가지고있다.

**LifeCycle은 리엑트의 순환구조라고 생각하면 된다.**

class 컴포넌트는 LifeCycle함수가 존재하며 function 컴포넌트는 hook를 사용한다.

지금은 class 컴포넌트의 LifeCycle을 알아보도록한다.

class LifeCycle은 총 9가지로 나뉘어있다.

크게 처음 will과 did로 구분하는데

1.  **will = 할 것이다.(미래형) // 어떤 작업을 하기 전에 수행함**
2.  **did = 했었다.(과거형) // 어떤 작업을 한 후 에 수행함**

으로 구분할 수 있다.

분류는 크게 3가지로 나뉘어있는데

1.  mount
2.  update
3.  unmount

이미지 참조

[##_Image|kage@dBReXB/btrmXFukZeB/WbYlAk64coJ8npWUdy48jK/img.jpg|CDM|1.3|{"originWidth":1674,"originHeight":917,"style":"alignCenter"}_##]

위와 같은 형식으로 나뉘어있으며 세부 사항을 살펴보겠습니다.

### Mount :

랜더링 된 이후 나타는 것을 마운트(Mount)라 한다.

초기 컴포넌트가 실행되면서 컴포넌트에서 가지고있던 props, state를 불러와 componentWillMount를 호출 한 후

DOM구조를 랜더링 합니다. 그리고 랜러딩이 완료된 이후 componentDidMount를 실행합니다.

### Update :

컴포넌트의 데이터의 변화를 감지하고 업데이트를 할 때 호출하는 메소드입니다.

데이터의 값으로는 크게

1\. props 변경

2\. state 변경

3\. 부모 컴포넌트 리랜더링 등이 있습니다.

순차적인 실행으로는

props 데이터변화 -> props값을 state와 동기화 -> state 변화 ->

shouldComponentUpdate : boolean값을 통하여 리랜더링 결정의 여부를 확인합니다.

만약 false라면 사이클을 마칩니다. -> 업데이트 호출 -> 랜더링의 순입니다.

### Update :

컴포넌트를 DOM에서 제거할 때 호출하는 메소드입니다.

1.  언마운트
2.  componentWillUnmount: 컴포넌트가 웹 브라우저 상에서 사라지기 전에 호출

쓰는 부분으로는 브라우저에 남아있는 중요한 정보를 지우기 위해서나 이벤트등을 제거하기 위하여 많이 쓰입니다.

### 함수형 리엑트에서는? :

class형 리엑트에서는 cycle 메소드를 제공하고 있으나

함수형 리엑트에서는 useEffect hook를 제공하고있습니다.

사용법은 조금은 다르지만 구글링을 통하여 충분한 자료를 얻을수 있습니다.
