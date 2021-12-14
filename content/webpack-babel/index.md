---
emoji: ✍️
title: [JS] 환경 설정 - webpack, babel
date: '2021-11-02 15:00:00'
author: me
tags: webpack babel 
categories: webpack babel
---

## Babel 설정

Babel 은 Javascript Transfiler 특정 타겟 버전으로 코드를 변환해줍니다.

⇒ 코드를 변환하는 역할 ex) ES6 이상의 문법을 ES5 아래의 문법으로 변경

필요한 모듈

- babel/core: 바벨이 동작하기 위해 필요한 핵심 모듈
- preset: 규칙을 담고 있는 상자

```
$ npm i -D @babel/core @babel/preset-env
```

.babelrc

어떻게 변화를 시킬지에 대한 정보를 바벨에게 알려주어야합니다.

```
{
  "presets": ["@babel/preset-env"]
}
```

## Webpack 설정

webpack 은 모듈 번들러입니다.

JS 파일들을 묶거나 난독화를 이용하여 압축하는등의 일을 할 수 있게 도와줍니다.

필요한 모듈

- babel-loader: webpack 은 자바스크립트 파일 단위를 다루는 모듈 번들러입니다. 코드를 읽고 변환하는 역할은 babel 이 담당 할 수 있도록, babel-loader 를 함께 사용합니다.
- webpack-cli: 커맨드 라인에서 웹팩 명령어를 사용 할 수 있게 도와줍니다.
- webpack-dev-server: 개발환경을 편하게 만들어주는 개발 서버

```
$ npm i -D webpack webpack-cli webpack-dev-server babel-loader
```

webpack.config.js

- entry: 빌드의 시작점
- output: 빌드된 파일이 어디에 위치할지
- module: 웹팩의 동작에 도움을 주는 loader 들이 위치 (ex. babel-loader, style-loader 등)
- extensions: import 시 생략가능한 확장자들을 정의

```
const path = require('path')

module.exports = {
  entry: path.resolve(__dirname, './src'), // src 내부의 index.js 를 바라본다
  output: { // 빌드한 결과물을 어디에 생성할 것 인가
    filename: 'bundle.[hash].js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [ // 어떤 파일들을 어떤 loader 를 이용하여 해석 할 것 인가
      {
        test: /\.(js)$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      },
    ],
  },
  resolve: {
    extensions: ['.js'], // .js 확장자 생략 가능
  },
}
```

build script 등록

```
"scripts": {
  "build": "webpack"
}
```

## HTML 설정

public/index.html

```
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>JS App</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

src/index.js

```
console.log('hello JS')
```

build 시 마다 hash 값이 변경된다. 매번 html 파일에 넣어주는건 무리가 있기 때문에 webpack plugin 의 힘을 빌린다.

```
$ npm i -D html-webpack-plugin
```

```
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: path.resolve(__dirname, './src'),
  output: {
    filename: 'bundle.[hash].js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.(js)$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      },
    ],
  },
  resolve: {
    extensions: ['.js'],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html',
    }),
  ],
}
```

## Dev Server

개발시 수정 후 매번 빌드를 하여 확인하는건 비효율적이다.

webpack-dev-server 를 이용하여 이를 간편하게한다.

```
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: path.resolve(__dirname, './src'),
  output: {
    filename: 'bundle.[hash].js',
    path: path.resolve(__dirname, 'dist'),
    publicPath: '/',
  },
  module: {
    rules: [
      {
        test: /\.(js)$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      },
    ],
  },
  resolve: {
    extensions: ['.js'],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html', // 기준이되는 html 파일
      filename: './index.html', // 빌드 후 사용할 html 파일 이름
    }),
  ],
  devServer: {
    contentBase: path.resolve(__dirname, 'dist'),
    inline: true,
    hot: true,
    open: true
  },
}
```

```
"scripts": {
    "lint": "eslint .",
    "prettier": "prettier .",
    "build": "webpack --mode production",
    "dev": "webpack serve"
}
```
