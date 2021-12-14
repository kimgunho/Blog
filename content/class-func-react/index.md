---
emoji: ✍️
title: class VS function
date: '2021-10-14 15:00:00'
author: me
tags: React class function
categories: React
---

### 리엑트는 컴포넌트 구성으로 이루어진다.

리엑트의 class방식과 function방식이 보여지는 표현과 동작은 같지만

과정의 차이는 있다.

예전에는 class형만이 할수있는 부분이있었지만

지금은 hook등의 출현으로 그 차이를 극복했다고한다.

이제 그 차이를 알아보자

### class

1.  state를 정의할수있다.
2.  랜더링은 반드시 render함수를 사용하여 구조를 표현한다.
3.  react lifeCycle에 정의된 함수를 사용할수있다. // lifeCycle의 대한부분은 [링크참조](https://www.notion.so/LifeCycle-17c2e183a33e40f59c3ec4d07609bb09)
4.  construtor를 사용하여 기존 es6 protoType 문법을 사용할수있다.

### function

1.  state와 lifeCycle를 사용할 수 없었지만 현재는 hook로 가능하게 되었다. // hook에 대한부분은 [링크참조](https://www.notion.so/HOOK-7eb862cc3306417c8a7cddcf6059b053)
2.  클래스형보다는 적은 메모리사용량으로 인하여 간단한 작업을 주로 이루었었다. (선언도 더쉽다.)
3.  일반함수, 화살표함수를 사용할때 각 this의 값은 다르다.

**일반 함수는 자신이 해당 this가 포함된 객체를 this로 가르킨다.**

**화살표 함수는 자신이 포함된 함수 데이터를 가르킨다.**

두방식 모두가 중요하게 느껴진다. 앞으로 더욱 두 방식을 번갈아 사용해보며 그 미묘한 차이에 대해 안 부분에 대해서 좀더 서술해나갈 예정이다.
