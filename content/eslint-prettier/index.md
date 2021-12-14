---
emoji: ✍️
title: ESLINT, PRETTIER - JS
date: '2021-11-01 15:00:00'
author: me
tags: ESLINT PRETTIER
categories: ESLINT PRETTIER
---

## ESLINT

약속된 코드의 규칙을 검사합니다.

필요한 모듈을 아래와 같습니다.

```
$ npm i -D babel-eslint eslint eslint-plugin-import eslint-plugin-node eslint-plugin-promise eslint-plugin-standard
```

.eslintrc 라는 설정 파일이 필요합니다

- parser: eslint 의 입맛에 맞게 코드를 읽고 변환해준다.
- parserOptions: 읽어올 코드에 대한 정보
- plugins: 기본 규칙외에 부가적인 규칙
- env: 개발할 코드의 환경
- extends: 베이스가 되는 룰
- rules: 유저가 정의한 룰

```
{
  "parser": "babel-eslint",
  "parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module"
  },
  "plugins": ["import", "node", "promise", "standard"],
  "env": {
    "browser": true,
    "node": true,
  },
  "extends": [
    "eslint:recommended",
  ],
  "rules": {
    "no-console": "error"
  }
}
```

기본 추천룰 ("eslint:recommended") + rules 에 기반하여 코드를 검사합니다.

no-console 규칙에 어긋나는 코드에 빨간줄을 그어줍니다.

또한 cli 명령어를 이용하여 검증이 가능합니다.

**package.json**

```
"scripts": {
    "lint": "eslint ."
}
```

## Prettier

코드를 이쁘게 포매팅해줍니다.

ESLint 와 함께 사용하면 규칙에 맞게 코드를 변경해주며 이쁘게 포맷팅해줍니다.

```
$ npm i -D eslint-config-prettier eslint-plugin-prettier prettier
```

.eslintrc 에 eslint-plugin-prettier 정보를 추가합니다.

```
{
  "parser": "babel-eslint",
  "parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module"
  },
  "plugins": ["import", "node", "promise", "standard"],
  "env": {
    "browser": true,
    "node": true,
  },
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ],
  "rules": {
    "no-console": "error"
  }
}
```

.prettierrc 라는 설정 파일이 필요합니다

```
{
  "useTabs": false,
  "printWidth": 80,
  "tabWidth": 2,
  "singleQuote": true,
  "trailingComma": "all",
  "endOfLine": "lf",
  "semi": false,
  "arrowParens": "always"
}
```

prettier 또한 cli 명령어를 이용하여 실행이 가능합니다

```
"scripts": {
    "lint": "eslint .",
    "prettier": "prettier ."
},
```

```
$ npm run prettier
```

**vscode auto fix**

```
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.eol": "\n",
}
```

## 불필요한 파일들 lint, prettier 제외하기

node_modules 나 dist 폴더의 경우 이미 검증된 코드들 이기 때문에 검증이 필요 없습니다.

.eslintignore

.prettierignore
