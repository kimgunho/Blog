---
emoji: ✍️
title: 면접후기 매꾸기
date: '2021-12-16 18:00:00'
author: me
tags: 면접후기 질문기록
categories: 면접후기
---

# 면접 후기록 및 질문 리스트 재복습

면접을 본 후 기록을 위한 포스팅입니다.

결과가 나오기 전까지 안절부절하는 마음을 부여잡고
면접을 보며 받은 질문들중 기술부분으로 상세하게 정리하려합니다.

## 1. 호이스팅이란 무엇인가?

우선 호이스팅을 알기전 자바스크립트의 변수들을 알아보자

- var
- let
- const
  이렇게 3가지 형태의 변수가 존재한다.
  var는 중복으로 선언이 가능하다. 예를들어

```
var name = 'kimchi';
console.log(name) // kimchi
var name = 'dragon';
console.log(name) // dragon
```

이렇게 선언 후 다시 재 선언을 하여도 실행에 지장이없다.
하지만 let과 const는 재선언이 불가능하다.

```
let name = 'kim';
console.log(name) // kim
let name = 'lee' // error
// const 동일
```

이렇게 var를 제외하고는 변수들은 재선언이 불가능하다.

여기서 var를 예시로 호이스팅을 알수있다.
**var는 선언하기전에 사용이 가능하다.**
예를들어

```
console.log(name); // undefined
var name = 'kim';
```

이렇게 선언이 가능하며 에러가 나지않는다. 이는 사실 아래와 같이 동작하기 때문이다.

```
var name;
console.log(name); // undefined
var name = 'kim';
```

**위의 상태처럼 var는 사용 후 선언하여도 최상단에 이미 선언이 되어있다. 이를 우리는 호이스팅이라 부른다.**<br />
또다른 말로는 **스코프 내부 어디서든 변수 선언은 최상위에 선언된 것 처럼 행동하는것을 말한다.**

그렇다면 let과 const는 호이스팅이 되지않는걸까?
결론은 아니다.
호이스팅은 되지만 에러가 나는 이유는 TDZ(Temporal Dead Zone)떄문이다.

let과 const는 var와 다르게 TDZ존의 영향을 받는다.

```
------- // DTZ존 start // 이곳 안에 있는 변수들은 사용을 할수없다. 그래서 에러가 난다.
console.log(name); // 에러

------- // TDZ존 end
const name = 'kim'; // 선언 및 할당
console.log(name) // 사용가능
```

## 그래서 let과 const를 사용하여야 하는 이유는

이로서 코드를 예측가능하게 하고 잠재적인 버그를 잡을수 있다.
그렇기에 우리들은 let과 const를 사용하여야한다.

변수는 3단계의 생성과정을 거친다.

1. 선언 단계
2. 초기화 단계
3. 할당 단계

- var는 **선언과 초기화가 동시에 일어난 후 할당 단계를 거친다.**
- let은 **선언 -> 초기화 -> 할당**
- const는 **선언과 초기화와 할당이 동시에 진행된다.**

## null과 undefined의 차이

사실 너무 쉽게 생각했는데 전달을 이상하게 한것같다.
면접관분들에게 null은 값이 없는 형태,
undefined는 값이 있으나 비어있는 상태라는 답변을 했다.
면접을 마친 후 다시한번 바로알아보았는데
정의는 이렇다.

- undefined 은 변수를 선언하고 **값을 할당하지 않은 상태**
- null은 변수를 선언하고 **빈 값을 할당**한 상태

보고 난 후 아..싶었다.
내가 먼가 착각을 한거같다.
너무 창피해서 면접을 끝내고 집으로 오는길에 자책만 한거같다.하하...
다시는 기초적이고 바보같은 실수하지말도록 상기시켜야한다. 이불킥이다.

## 배열 메서드 정리

### 개인적으로 자주 쓴 메서드

- map : 배열의 각 원소별로 지정된 함수를 실행한 결과로 구성된 새로운 **배열을 반환**한다.
- reduce : 누산기(accumulator) 및 배열의 각 값(좌에서 우로)에 대해 (누산된) 한 값으로 줄도록 함수를 적용(잘쓰면 엄청 좋음)
- find : 특정 결과에 대하여 true인 데이터만 반환한다.
- filter : 지정된 함수의 결과 값을 true로 만드는 원소들로만 구성된 별도의 배열을 반환한다.
- splice : 배열의 특정위치에 요소를 추가하거나 삭제 // splice(index, 제거할 요소 개수, 배열에 추가될 요소)
- push : 배열 뒷부분에 값을 삽입
- forEach : 배열의 각 원소별로 지정된 함수를 실행한다. 하지만 리턴값을 반환하지는 않는다.

**배열함수는 아니지만 배열로 만들기 좋은 함수**

- split(매개체) : 매개체를 기준으로 여러개의 배열로 나눈다. 유용함

### 알고는 있지만 자주 활용하진 않았던 메서드

- pop : 배열 뒷부분의 값을 삭제
- unshift : 배열 앞부분에 값을 삽입
- shift : 배열 앞부분의 값을 삭제
- concat : 다수의 배열을 합치고 병합된 배열의 사본을 반환
- every : 배열의 모든 요소가 제공한 함수로 구현된 테스트를 통과하는지를 테스트
- some : 지정된 함수의 결과가 true일 때까지 배열의 각 원소를 반복
- reverse : 배열의 원소 순서를 거꾸로 바꾼다.
- sort : 배열의 원소를 알파벳순으로, 또는 지정된 함수에 따른 순서로 정렬한다. 모든 원소를 문자열로 취급해 사전적으로 정렬
- toString : 배열을 문자열로 바꾸어 반환한다
- valueOf : toString과 비슷, 그러나 배열을 반환
- join : 배열 원소 전부를 하나의 문자열로 합친다.

```
var arr =[ 1, 2, 3, 4 ];
console.log( arr.join() );      // 1,2,3,4
console.log( arr.join( '-' ) ); // 1-2-3-4

```

- slice : startIndex부터 endIndex까지에 대한 새로운 배열 반환 //

```
var arr = [ 1(0), 2(1), 3(2), 4(3), 5(4), 6(5), 7(6) ];
var newArr = arr.slice( 3, 6 );
console.log( 'slice',  newArr ); // [ 4, 5, 6 ]
```

## 이벤트 버블링, 캡처링, 위임

우선 여기서 말하는 이벤트는 사용자의 입력을 받기 위한 기능을 말합니다.

### 이벤트 버블링

이벤트 버블링은 **특정 화면 요소에서 이벤트가 발생하였을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 오름 특성**을 말합니다.
예시를 코드로 확인해 보겠습니다.

```
<div class='first'>
  <div class='second'>
    <div class='last'>
    </div>
  </div>
</div>

const divs = document.querySelectorAll('div');
divs.forEach((ele)=> {
  ele.addEventListener('click', (event)=> {
    const {currentTarget : {className}} = event;
    console.log(className)
  })
})

```

위 코드는 최하위 div를 클릭하면 first, second, last 까지 출력되고 first만 클릭한다면 first만 출력됩니다.
그런데 왜 하위엘리먼트를 클릭하면 전체가 다 출력되는가에 대한 의문이 있습니다.

이에 대한 이유는 브라우저가 이벤트를 감지하는 방식 때문입니다.
**브라우저는 특정 엘리먼트에서 이벤트가 발생했을 때 그 이벤트를 최상위에 있는 화면 요소까지 이벤트를 전파시킵니다.**

우리는 이와 같은 하위에서 상위 요소로의 이벤트 전파 방식을 **이벤트 버블링(Event Bubbling)**이라고 합니다.

### 이벤트 캡쳐

이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 내림 이벤트 전파 방식입니다.

코드를 확인해본다면

```
<div class='first'>
  <div class='second'>
    <div class='last'>
    </div>
  </div>
</div>

const divs = document.querySelectorAll('div');
divs.forEach((ele)=> {
  ele.addEventListener('click', (event)=> {
    const {currentTarget : {className}} = event;
    console.log(className)
  },{
    capture : true // default = false입니다.
  })
})

```

위 코드에서 처럼 addEventListener에 옵션객체로 capture : true로 설정해주면 됩니다.
그러면 해당 이벤트를 감지하기 위하여 브라우저는 이벤트 버블링과 반대방향으로 탐색을 시작합니다.

### 이벤트 위임

이벤트 위임은 하위요소들에게 각각 이벤트를 핸들링 하지않고 상위 요소한곳에 이벤트를 주어 하위 요소를 핸들링 하는 기법을 말합니다.
이 방법은 실제 실무에서 많이 쓰이는 방식이기도 하기 때문에 매우 중요하게 생각하고 있습니다.

아래 코드를 보며 패턴을 익혀봅시다.

```
<ul class="itemList">
	<li>
		<input type="checkbox" id="item1">
		<label for="item1">이벤트 버블링 학습</label>
	</li>
	<li>
		<input type="checkbox" id="item2">
		<label for="item2">이벤트 캡쳐 학습</label>
	</li>
</ul>

const itemList = document.querySelector('.itemList');

itemList.addEventListener('click', (event)=> {
  console.log('clicked');
})
```

위 방식처럼 li엘리먼트 각각에 이벤트를 주는 방식이 아닌 상위 요소에 이벤트핸들링을 주어 하위요소까지 함께 제어하는 기법을
이벤트 위임이라 명칭합니다.

## 얕은 복사

JS의 값은 원시값과 참조값으로 구분됩니다.
원시값 = Number, String, Boolean, Null, Undefined
참조값 = object, symbol, array, function

원시값은 값을 복사하는 경우 복사된 값을 다른 메모리에 할당하기 때문에 원래의 값과 복사된 값이 서로 영향을 주지않습니다.

```
const a = 1;
let b = a;
b = 2;

console.log(a) // 1
console.log(b) // 2

```

하지만 **참조값은 변수가 객체의 순수한 원시데이터가 아닌 주소를 가르치는 값이기에** 복사된 값을 나타냅니다.

```
const a = {number: 1};
let b = a;

b.number = 2

console.log(a); // {number: 2}
console.log(b); // {number: 2}
```

이러한 특징때문에 참조값을 복사하는 방법은 크게 2가지로 나뉩니다.

## 깊은 복사

위와 같이 참조값을 그대로 복사한 데이터의 경우 앝은 복사때문에 원래의 데이터가 의도치않게 변동이 되어 버립니다.
그렇기에 이러한 부분을 해결하기 위한 깊은 복사를 사용해야하는 경우가 있습니다.

깊은 복사를 하는 방식을 알아보겠습니다.

### Object.assign()

우선적으로 Object.assign()을 사용하여 깊은 복사를 사용하는 방법이 있습니다.
이방법은 assign({}. 복사할 객체) <- 이러한 형식을 이용하여 복사를 할 수 있습니다.
코드로 확인해보겠습니다.

```
const me = {
  name : 'kim'
  age : 30
}

let secondMe = Object.assign({}, me)
secondMe.name = 'lee'

console.log(me) // name kim, age 30
console.log(secondMe) // name : lee, age 30
```

위 코드를 확인하면 초기 me객체형식은 원본값을 유지하고
새롭게 복사한 secondMe 데이터는 원본 객체에서 name값만 변경된 상태로 유지됩니다.

**단 위 Object.assign의 방식을 사용할 경우 depth깊이가 2 이상인 경우(객체안에 객체) 얕은 복사가 됩니다.**

### 전개연산자

이방법은 좀더 개인적으로 좀더 수월한 방식인데
스프레드 연산자를 사용하는 방법입니다.

코드로 확인해보겠습니다.

```
const me = {name : 'kim'}

const newMe = {...me}
newMe.name = 'pack'
console.log(me) // name : kim
console.log(newMe) // name : pack
```

이처럼 전개연산자를 사용해서 깊은 복사를 구현할 수도 있습니다.
리엑트에서 배열을 주로 다룰때 const a = [...array]와 같은 방법입니다.

하지만 **전개연산자의 방식도 Object.assign과 동일하게 뎁스가 2이상일 경우 얕은 복사가 발생합니다.**

### JSON.stringify(), JSON.parse()

만약 복사를 하여야하는 객체가 json구조라면 JSON.stringify(), JSON.parse()를 사용하여 깊은 복사를 구현할 수 있습니다.
그전에 JSON.stringify(), JSON.parse()를 설명하자면

- JSON.stringify() : json의 구조를 String화 시키는것입니다.
- JSON.parse() : String화 되어있는 json구조를 json형태로 변경하는것입니다.

String구조는 원시타입 데이터이기 때문에 주소(참조)가 아닌 순수 값자체를 복사할 수 있습니다.

```
const myObj = {
  a: 1,
  b: {
    c: 2,
  },
};

const newObj = JSON.parse(JSON.stringify(myObj));

newObj.b.c = 3;

console.log(myObj.b.c); // 2
console.log(newObj.b.c);  // 3
console.log(myObj=== newObj); // false
```

**단 위 방식은 json구조만 가능하며, 함수는 undefined로 반환되기에 복사가 되지않습니다. 또한 타방법에 비해 속도가 느린편이라고 합니다.**

### lodash 모듈 사용

lodash 모듈을 사용하는 방법으로서 npm install lodash를 설치해줍니다.
이후 require와 cloneDeep 함수를 사용하는 방법이있습니다.

자세한 부분은 코드로 확인하겠습니다.

```
const lodash = require('lodash'); // require를 lodash변수에 할당합니다.

const myTest = {
  a : 1,
  b : {
    c : 2,
    d : 3
  },
  func : ()=> {
    return this.a
  }
}

const newTest = lodash.cloeDeep(myTest);

```

이방법은 앞서 설명드린 3가지 단점을 모두 보완할 수 있습니다. 뎁스와 함수에 대한부분까지 적용이 가능합니다.
하지만 lodash 모듈을 설치해야 사용할 수 있습니다.

## 클로져

이론적으로 클로저란 **함수와 함수가 선언된 어휘적(렉시컬) 환경의 조합** 이라고 소개하고있습니다.
다른말로는 함수를 칭하고 그 함수가 선언된 환경과의 관계라는 개념이라는 글을 보았습니다.

사실 정의는 각 소개를 하시는 분들마다 조금식 다르기 때문에 코드를 보며 확인해보겠습니다.

```
function outter(){
  const name = 'kim'
  function inner(){
    console.log(name)
  }
  inner();
}

const myFunc = outter();
myFunc(); // console kim

```

일반적으로 함수안의 지역 변수들은 해당 함수가 처리되는 동안에만 존재하기 때문에 inner함수가 실행되면 name변수에 접근 할 수 없는걸로 알수있습니다.
하지만 js에서는 함수를 리턴할 때, 클로저를 형성하기 때문에 당시 함수와 함수가 선언된 환경의 조합(로직)을 기억합니다.

그렇기때문에 만약 myFunc가 아닌 다른 변수로 outter를 할당하여 선언하여도 클로저에 의하여 기억하고 있으므로
재사용이 가능합니다.

이처럼 제가 생각하는 클로저란
**재사용이 가능한 함수의 독립적인 환경을 기억하는 함수**라고 생각합니다.
