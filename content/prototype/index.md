---
emoji: ✍️
title: prototype, chain
date: '2021-10-18 15:00:00'
author: me
tags: javaScript prototype chain
categories: prototype
---

창피한 일이지만 퍼블리셔로 일을 하는 동안에는 자바스크립트의 프로토타입을 사용할 일이 거의 없었다.

이러한 시간이 지남에 따라 점점 나 자신이 퇴화되는 기분이 들어 자괴감까지 들기 시작하려는 찰나에

다시한번 새로운 마음가짐으로 복습을 해보기로했다.

### 1\. prototype이란 무엇인가?

자바스크립트라는 언어를 공부하면 할수록 모든 변수,함수,배열,객체등등 모든것이 상호작용하는

친구이자 부모라는 생각이 들었다. 객체지향언어의 특징이기도 하지만

그중 프로토타입이란 개념은 쉽게 생각하면 쉬울수있고 어렵게 생각하면 어려울수있는거같다.

정리할부분이 너무 많지만 하나하나 시작해보자

```
function 부모님(){...}

부모님.prototype.firstName = 'kim'

const 자식 = new 부모님

console.log(자식.firstName)//'kim'
```

위 코드를 보면 자식에게는 firstName이라는 속성을 준 적이없다.

하지만 자식의 firstName을 출력한다면 'kim'이 출력된다.

이는 자식이 name을 가지고 있지않아도 상속의 개념으로서 부모님속성이 만약 firstName을 가지고있다면

해당 값을 거슬러 오르면서 찾는것이다.

여기서 새롭게 안 사실이 있었는데

우리들이 쓰는 많은 내장함수들이 있다. 그중에서 예시로 배열의 map()함수를 보면 우리는

map()이라는 함수를 만든적이없다. 아래 코드를 보자

```
const arr = [1,2,3,4,5]
const arr = new array(1,2,3,4,5)
// 위 두 코드는 보여지는 방식은 다르지만 같은 뜻이다.

array.prototype.map()
//위 코드는 MDN에서 map()에 대한 함수를 소개할때 표시되는 정식 이름이다.

arr.map((item)=> {
    ...
})
//사용할 때
```

이해가 잘 될지 모르겠지만

위 코드를 보면 결국 자바스크립트엔진에는 array.prototype으로 map()이라는 함수를 미리 만들어놓은것이다.

const arr은 new array와 같은 배열이기 때문에

미리 만들어 놓은 map()함수를 상속받아 사용할수 있는것이다.

- _참고이미지_

[##_Image|kage@EDXrf/btrhtXGGa3L/moPIeoUZ5bTTyZ8CEZICCK/img.png|CDM|1.3|{"originWidth":697,"originHeight":570,"style":"alignCenter","filename":"스크린샷 2021-09-30 01.44.57.png"}_##]

### 2\. prototype chain

위 내용처럼 프로토타입은 연계되며 상속된다.

여기서 상속이라는 의미는 프로토타입 체이닝으로 발전이된다.

```
function big(){...}
big.prototype.size = 100

function middle(){}
middle.prototype = new big()

function small(){}
small.prototype = new middle()

const val = new small()
console.log(val.size)// 100
```

위 코드는

1.  big함수 생성후 사이즈는 100으로 추가
2.  middle함수 생성후 빅함수를 프로토타입으로 연결
3.  스몰함수 생성후 미들함수로 프로토타입 연결
4.  val변수에 생성자 함수(스몰) 할당
5.  val.size를 출력시 100이 출력됨

위 과정을 나타낸다. val.size를 출력하려는 순간 val에서 size를 찾은 후 만약 없다면

그 위 부모단계인 미들, 만약 미들에도 없다면 빅까지 찾아간다. **이렇게 연계되어 상속의 최상단까지**

**데이터를 찾아주는 것을 우리는 prototype chain으로 부른다.**
