---
emoji: ✍️
title: 자식에서 부모로 데이터 전달 - React
date: '2021-11-03 15:00:00'
author: me
tags: React props state
categories: React
---

### 리엑트 자식컴퍼넌트 요소에서 부모컴포넌트로 데이터 전달

리엑트의 특성중 state는 위에서 아래로 부모에서 자식에게로 전달되며 그 반대는 되지않는다.

그렇기에 그 반대의 경우는 어떻게 하여야하는지

알아보자 이 방법은 class방식에서 진행한 방식이다

```
// in parent class
state = {
	stateData = null
}

getData = (data) => {
	this.setState({
		stateData = data
	})
}

render(){
	return(
		<Compo data={this.getData}/>
	)
}



// in child class
render(){
	return(
		<div>
			<button onClick={this.props.data}>click me</button> // 매개변수가 없다면
			<button onClick={()=> this.props.data(매개변수)}>click me</button> // 매개변수가 있다면
		</div>
	)
}
```

위와 같은 방식의 함수로 가능하다.

풀이를 하자면

부모클래스에 데이터를 업데이트(setState) 하는 함수를 선언한다.

이후 역할을 할 자식 컴포넌트에게 함수를 넘겨준다.

자식 컴포넌트에서 특정조건 또는 일반 실행 시 전달해올 Props값을 실행한 후

함수안에 setState를 사용하여 부모 class안에 state의 데이터를 업데이트한 후 불러온다.

이후 주의할 점은 불러오기전에 랜더링된 무언가 있다면 비동기화 시켜준 후 이후 처리할 값을 실행한다.
