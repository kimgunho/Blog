---
emoji: ✍️
title: 비동기를 동기처럼 바꾸는 방법 3가지
date: '2021-10-17 15:00:00'
author: me
tags: asynchrony synchronicity
categories: 비동기
---

### 1\. 비동기와 동기의 구분

자바스크립트는 기본적으로 동기언어이다.

동기란 호이스팅의 순서대로 위에서부터 아래순서로 순차적인 실행방식을 말한다.

```
console.log('1')
console.log('2')
console.log('3')

//1
//2
//3
```

비동기처리는 동기처리의 반대로 특정 로직이 끝날때까지 기다리지않고 먼저 실행할수 있는 부분은 처리하는 방식이다.

예를들면 서버에서 필요한 데이터를 요청한 후 그 데이터를 기반으로 작업을 실행할 시

서버가 해당 요청에 대한 응답을 줄때까지 마냥 기다릴순 없기 때문이다.

```
console.log('1')
settimeout(()=> console.log('2'),1000)
console.log('3')

//1
//3
//2 (1초후)
```

위와 같은 상황을 말한다.

### 2\. 동기를 비동기로 처리하는 방식 #1 \[ callback \]

콜백이란 사실 뜻하지않게 많이 사용해온 방식이다.

map(), foreach(), 많은 내장함수를 사용할 때 우리들은 아래처럼 사용할 것이다.

```
const num = [1,2,3,4]

num.map((item)=> console.log(item))
```

위 map이라는 함수를 사용할 때 인자로 새로운 함수를 선언하였다.

이는 함수안에 새로운 함수를 넣어 map을 실행함과 동시에 인자안에 새로운 함수를 실행하는

call back 다시부르는 함수이다. 위같은 방식은 사실 동기적 콜백이다

콜백이 어떤것인지 알았으니 비동기적 콜백을 알아보자

```
function print(callback){
    setTimeout(callback, 1000)
    console.log('제이름은...')
}

print(()=> console.log('홍길동 입니다.'))
```

위 코드처럼 프린트를 실행한순간 인자값으로 '홍길동입니다.' 를 불러온다.

하지만 setTitmeout, 1초후 불러오는 방식을 사용하여

'제 이름은' 이 먼저 출력된 후 1초가 지나서 콜백함수 홍길동입니다가 불러지는 것이다.

하지만 콜백을 구글링하면 많은 사람들이 표현하듯이 콜백 = "콜백지옥" 이라는 단점이있다.

콜백지옥은 작업자와 다른 실무자가 코드를 확인했을때 좋지않은 가독성으로 인하여

어려운 유지보수, 작업이해등이 존재한다.

콜백지옥의 예시를 보자

```
function add(x, callback){
    let sum = x * x
    console.log(sum)
    callback(sum)
}

add(2, (val)=>{
    add(val, (val)=>{
        add(val, (val)=> {
            add(val, (val)=> {
                console.log('end...')
            })
        })
    })
})

//4
//6
//256
//65536
//end...
```

물론 위와 같은 코드를 짤 일은 없겠지만 예시를 위해 보여지는 것이다.

위와같이 함수선언 후 실행⇒ 콜백 ⇒ 실행 ⇒ 콜백 ⇒ 실행 ⇒ 콜백...

이런 과정을 거치다보면 한눈에 보기에도 어렵고 이게뭐야.. 라는 말이 나올정도로

눈이 아프기시작한다. 그렇기에

**과도한 콜백은 지양하여야한다. —**

### 3\. 동기를 비동기로 처리하는 방식 #2 \[ Promise \]

콜백지옥을 해결하기위해 새롭게 나타난 방식이다.

_프로미스는 약속이라는 뜻인데 왜 하필 이름이 약속인지는 아직 의문이다._

우선 말로 서술하기전 코드를 본 이후 파해쳐보자

```
const sum = new Promise((resolve, reject)=> {
    resolve(2)
    reject(new Error('is err...'))
})

sum
.then(val=> val*val)
.then(val=> val*val)
.then(val=> console.log(val)) // 16
.catch((err)=> console.log(err))
.finally(()=> console.log('end...'))//end...

// 확실히 콜백지옥보다는 보기가 훨씬 간결하다.
```

코드를 풀어보자

프로미스는 사용시 반드시 new를 붙인 상태에서 사용한다.

또한 결과값의 성공(resolve)과 실패(reject)를 반환하게 되어있다.

로직의 의한 결과값이 resolve라면 실행후 리턴값에 해당하는값을

.then((리턴값)⇒ {로직의 의한 리턴값 재 생성})

위 then의 인자에 초기 sum의 리턴값을 배출하여 이후 로직과 로직에 의한 리턴값을 재선언할수있다.

이후 반복적으로 배출→선언→도출이라는 결과로 나타낼수있다.

만약 로직의 과정에서 문제가 발생하여 에러가 도출된다면 reject를 배출하게 되어있다.

좀더 정확한 에러와 손쉬운 수정을 위하여 상세한 실패값에 대한 내용을 미리 작성하여 방지할수있다.

reject에 의한 실패값은 .catch()에서 재 선언할수있다.

만약 then에서 에러가 난다면 이후 then은 실행되지않고 catch로 넘어간다.

.finally()는 성공을 하든 실패를 하든 최종적으로 실행하는 선언값을 실행할수있다.

- Promise.all\[\] // 실행하고자하는 모든 프로미스를 실행한 후 결과값은 순위를 따지지않고 한번에 도출되는 메소드이다.

```
const promise1 = new Promise((res, rej)=> {
    res(1)
})
const promise2 = new Promise((res, rej)=> {
    res(2)
})
const promise3 = new Promise((res, rej)=> {
    res(3)
})

Promise.all([promise1, promise2, promise3])
.then((val)=> console.log(val)) // [1, 2, 3]
```

위 코드와 같이 Promise.all은 여러 프로미스의 실행값을 동시에 반환한다.

- Promise.race(\[\]) // 이는 레이스 말 그대로 가장먼저 완료된 결과값의 순서대로 반환한다.

### 4\. 동기를 비동기로 처리하는 방식 #3 \[ async, await \]

일전 포켓몬 api를 이용하여 포켓몬도감을 만드는 과정에서

fetch이후 promise를 이용한 방식으로 res값을 반환하여 데이터를 만든적이있었다.

그 코드를 본 후 선생님께서는 async, await를 사용하여 해보는걸 추천해주셔서

이 후 어씽크, 어웨잇을 사용해본 결과

프로미스보다 훨씬 보기좋은 가독성과 편리함을 알게되었는데 아래 코드를 보며 다시한번 이해해보자

```
// 우선 일전에 사용한 프로미스를 확인해보자

function detailInfo(info){
    return new Promise(res => {
        setTimeout(()=>{
            console.log(info)
            res(info)
        },1000)
    })
}

detailInfo('name')
.then((data)=>{
    return detailInfo('age')
})
.then((data)=>{
    return detailInfo('job')
})

// output : name age job
```

프로미스도 잘쓰면 좋지만 너무 과다한 사용은 가독성의 어려움과 어느정도의 스트레스를 불러올수있다.

만약 위 코드가 더 불러오는 정보가 많았다면 then파티가 되어있을것으로 예상된다.

자 그럼 이번엔 어씽크 어웨이를 사용한 코드를 보며 비교해보자

```
function detailInfo(info){
    return new Promise(res => {
        setTimeout(()=>{
            console.log(info)
            res(info)
        },1000)
    })
}

async function loadDetail(){
    const data1 = detailInfo('name')
    const data2 = detailInfo('age')
    const data3 = detailInfo('job')
}

loadDetail()
```

프로미스를 사용한 코드와 어씽크를 사용한 코드를 비교하면 확실히 어씽크가 좀더 보기좋고

높은 가독성을 나타내고있다. 위 코드는 입력한 정보를 1초후에 실행시키는 코드인데

loadDetail함수는 비동기로 실행되어 1초후에 실행되는 detailInfo함수가 동시에 진행되는것이다.

만약 data1이 할당이 끝난 후 data2가 할당되고 그 이후 data3이 할당되는 순서대로 진행을 원한다면

**await** 를 사용할수있다. 아래코드를 보자

```
async function loadDetail(){
    const data1 = await detailInfo('name')
    const data2 = await detailInfo('age')
    const data3 = await detailInfo('job')
}

loadDetail()
```

위와 같이 await를 붙인다면 앞서 적은듯이 await 최상단 data1부터 3까지 data1처리가 끝난 후 2처리, 3처리

순서대로 로직이 진행된다.
