---
emoji: ✍️
title: 변수, 호이스팅, TDZ 개념정리
date: '2021-10-19 15:00:00'
author: me
tags: 변수 호이스팅 TDZ
categories: Hoisting
---

### 1\. 변수의 종류와 특징

변수에는 3가지 ( var, let, const ) 종류가 있으며 각 변수 에는 각각의 차이점이 있습니다.

```
// 변수의 실행단계 1.선언, 2.초기화, 3.할당

var = {
    1. 선언과 동시에 초기화
    2. 할당 순서로 동작
    // 한번 선언된 변수를 다시 선언가능

}

let = {
    1. 선언
    2. 초기화
    3. 할당
}

const = {
    1. 선언과 초기화 할당 동시 시작
}

// var는 함수스코프지면 let, const는 블록스코프이다.
```

### 2\. 호이스팅

```
console.log(a) // undifided
var a = 'hello'
console.log(a) // hello
```

위 var a는 최초 선언되기전 출력시 값만 없는 상태로 출력이 가능하다.

이 후 a에 값을 할당 한다음 출력하면 해당 값이 출력된다.

사실 위 코드에서 var a는 최초 출력이전 상단에 출력이 된 상태이다.

아래 코드의 형태를 보자

```
var a;
console.log(a) // undifided
var a = 'hello'
console.log(a) // hello
```

위 코드의 형태대로 최초 선언을 하지않아도

**해당 스코프 범위 안에서 최상단으로 끌어올려 선언되는것을 우리는 호이스팅이라 명칭한다.**

function과 var는 선언되기전 출력을 하여도 자바스크립트 내부안에서 호이스팅이 가능하다.

### 3\. TDZ (Temporal Dead Zone)

```
a() // A or B의 값이 출력됨

function a(){
    console.log(name)
}

var name = 's'  // A : s출력
const name = 's' // B : err 위로는 TDZ
```

위 코드안에서 만약 name이 var를 사용했다면 s가 출력될 것이고

const를 사용했다면 에러가 나타날것이다.

const와 let은 TDZ가 존재하는데 이공간은 해당스코프의 시작지점부터 초기화 지점까지의 구간을 말한다.

const와 let은 선언 후 DTZ을 벗어날수있는데 이는 코드를 예측가능하게한다.

**정리하면 TDZ존은 말그대로 임시로 죽어있는 공간이라는 의미로서**

**변수(const, let)가 선언되기전 스코프범위 에서는 죽어버리는 공간이다.**

### 4\. 그렇다면 let과 const에서는 호이스팅이 일어나지않는것인가?

let과 const 역시 호이스팅이 가능하다. 하지만

위 두 변수들은 TDZ의 영향을 받기때문에

구문에러가 나타날것이다. (**클린한 코드를 위해 let과 const를 사용하자)**

또한 위에서 설명한 것들과 반대로 var, function 선언은 TDZ에 영향을 받지 않는다. 이것들은 현재 스코프에서 호이스팅 된다.
