---
emoji: ✍️
title: i18next 다국어 라이브러리 기록
date: '2021-10-15 15:00:00'
author: me
tags: i18next react 다국어
categories: i18next
---

이번에 진행중인 반응형웹 프로젝트를 진행하면서 해당 클라이언트측이 원하는 기능중

한.영 다국어 기능이 필요한 부분이있었다.

예전 php에서 사용하던 방식이 아닌

리엑트에 맞는 전용 다국어 라이브러리를 찾아보다

다행이 딱 원하는 기능을 제공하는 i18next 라이브러리를 찾았다.

이 블로그는 해당 기능으로 사용을 완료 후

잊지않기 위해 사용한 방법을 기록하기 위해 작성하는 것이다.

### 1\. 설치

```
$ npm i react-i18next i18next -S
```

좀더 디테일한 가이드는

**라이브러리 홈페이지 : [https://react.i18next.com/getting-started](https://react.i18next.com/getting-started) 를 참고하는것이 좋다.**

### 2\. 사용법

### **1\. 해당 기능을 위한 폴더와 파일을 생성해준다.**

사용을 위한 폴더의 파일 구조

```
├── lang [forder]
│ ├── i18n.js
│ └── lang.en.json
│ └── lang.ko.json
```

**2\. 초기 구조를 i18n.js 에서 잡고, 실제 번역 스크립트는 en, ko json 파일에 작성한다.**

i18n 설정 환경

```
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';

import langEn from './lang.en.json'; // 영문 json 파일
import langKo from './lang.ko.json'; // 한글 json 파일

// 기본 언어 설정
let lang = localStorage.getItem('@lang'); // 필자는 스토리지에 한영 state를 저장해두었다.
if (lang === null) { // 만약 lang의 기본값이 null이면 lang = ko
  lang = 'ko';
}

const resources = { // 리소스 파일 셋
  en: { translation: langEn },
  ko: { translation: langKo },
};

i18n.use(initReactI18next).init({ // 초기설정
  resources,
  lng: lang, // 위 스토리지로 설정한 lang을 불러온다.
  debug: false, // debug console.log 설정 트루면 페이지마다 i18n 환경을 불러온다.
  defaultNS: 'translation',
  ns: 'translation',
  keySeparator: '.', // for key in object[json] 객체안의 키의 설정을 위한 부분
  returnObjects: true, // for array in object[json] json에 배열을 사용하기 위한 부분
  interpolation: {
    escapeValue: false,
  },
  react: {
    useSuspense: false,
  },
});

export default i18n;
```

위 init설정을 마친 후

```
---EN
{
  "home": {
    "hiring": "HIRING",
    "welfare": "WELFARE",
    "company-category-01": "A message from the CEO",
    "company-category-02": "About the company",

---KO
{
  "home": {
    "hiring": "인재채용",
    "welfare": "복리후생",
    "company-category-01": "CEO 인사말",
    "company-category-02": "회사소개",
```

key는 동일하게 value는 해당 값에 맞도록 입력된 json파일을 설정한다.

만약 api를 불러오는 데이터였다면

json파일을 알맞게 구성해야할듯하다.

**3\. 이제 설정을 마쳤으면 사용을 해보자**

```
import React from 'react';
import { useTranslation } from 'react-i18next'; // 사용 임폴트

const Intro = () => {
  const { t, i18n } = useTranslation(); // 제공 api

// t : json 데이터에서 키값을 기준으로 불러올수 있다.
// i18n : i18n에서 제공해주는 api기능을 사용할수있다 ex : i18n.langage = 현재 언어상태 확인
// i18n.changeLanguage(lang) <- 언어변경 이벤트

  return (
    <div className={cx('container')}>
      <h3 className={cx('title')}>{t('people.intro-title')}</h3>
      <p className={cx('text')}>{t('people.intro-text')}</p>
    </div>
  );
};
```

사용을 하다보면 사실 그렇게 어렵게는 느껴지지 않을것이다.

기본 구조 이해만 잡히면 열심히 사용할 일만 있는거같다
