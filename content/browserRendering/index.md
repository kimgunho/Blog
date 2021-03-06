---
emoji: ✍️
title: 브라우저 렌더링 과정
date: '2021-10-15 15:00:00'
author: me
tags: rendering browser
categories: Browser
---

# 브라우저 렌더링 과정

각각의 브라우저들은 웹이 렌더링되는 과정에서 사용하는 고유 렌더링 엔진이 있습니다.

- 크롬, 웨일 : 블링크(Blink)
- 사파리 : 웹킷 (webket)
- 파이어폭스 : 게코 (Gecko)

## 렌더링 과정

클라이언트는 서버에서 HTML, CSS, JavaScript를 다운받습니다.

다운된 HTML, CSS로 Object Model을 만듭니다. HTML ⇒ DOM, CSS ⇒ CSSOM

**과정.1 HTML ⇒ DOM (document Object Model)**

![https://media.vlpt.us/images/st2702/post/a108f8e5-cfd2-4147-93ac-b1e3afdb9ae0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.13.png](https://media.vlpt.us/images/st2702/post/a108f8e5-cfd2-4147-93ac-b1e3afdb9ae0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.13.png)

**과정.2 CSS ⇒ CSSOM (CSS Object Model)**

![https://media.vlpt.us/images/st2702/post/58021275-6b4f-4ae6-b53e-fed935d5fcb7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.18.png](https://media.vlpt.us/images/st2702/post/58021275-6b4f-4ae6-b53e-fed935d5fcb7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.18.png)

**과정.3 DOM과 CSSOM을 합성하여 ⇒ Render Tree 생성**

![https://media.vlpt.us/images/st2702/post/143bd068-01f4-4c6c-90c1-8db21f3b6a6c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.15.03.png](https://media.vlpt.us/images/st2702/post/143bd068-01f4-4c6c-90c1-8db21f3b6a6c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.15.03.png)

**과정.4 레이아웃 혹은 Reflow 단계**

레이아웃 단계는 노출될 디바이스의 뷰포트(화면)내에서 랜더 트리가 정확한 위치와 크기를 계산하는 과정을 말합니다. 각 요소마다 상대적인 측정값들은 화면으로 옮겨지는것과 동시에 절대적인 px값으로 변하게 됩니다.

**과정.5 페인팅**

과정4까지 진행된 랜더 트리는 디스플레이에 페인팅되며 브라우저에 노출됩니다.

**과정.6 그렇다면 자바스크립트는 언제불러오는가?**

자바스크립트는 CSSOM과 마찬가지로 렌더링 엔진이 돌아가면서 스크립트 태그를 만나면 바로 자바스크립트 엔진으로 제어권을 넘겨버린다.

자바스크립트 엔진으로 넘겨진 스크립트는 아주 단순한 형태의 코드로 변환되어 돌아갑니다.

- 여담으로 스크립트는 항상 코드 제일 마지막에 삽입하라고 하는데 그 이유가 바로 돔이 생성되기전에 스크립트가 실행되면 에러 혹은 작동이 안될수 있기때문이다.

**이 모든것을 순서대로 정리를 해본다면**

1.  html의 구성을 읽어가면서 DOM트리를 구성합니다.
2.  CSS구성을 읽어가면서 CSSOM트리를 구성합니다.
3.  DOM과 CSSOM을 합쳐 렌더트리를 형성합니다.
4.  렌더트리에서의 구조를 뷰포트내의 상대적값을 절대적값으로 변경하여 형태를 나타냅니다.
5.  정리된 노드를 화면에 출력합니다.

위 정리된 순서대로 브라우저는 웹을 렌더링하게 됩니다.

이미지 출저 : [https://bit.ly/2WochoN](https://bit.ly/2WochoN)
