---
title: "Gatsby.js 스타일링"
date: "2020-08-27"
template: "post"
draft: false
slug: "getsby-tutorial-styling"
category: "Web Development"
tags:
  - "gatsby"
description: "Gatsby.js 스타일링"
socialImage: ""
---

## 학습 목표

- 사이트 구축을 위한 React 구성 요소에 대해 자세히 알아보기
- Gatsby 웹 사이트 스타일 지정 요소에 대해 자세히 알아보기
  
## 글로벌 스타일 사용

- 사이트의 전체적 느낌 설정.

## 실습용 새 사이트 만들기

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

프로젝트 구조

```TEXT
├── package.json
├── src
│   └── pages
│       └── index.js

```

## CSS 파일에 스타일 추가

1. `css` 파일 만들기

```shell
cd src
mkdir styles
cd styles
touch global.css
```

다음과 같은 구조 생성

```TEXT
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css

```

2. `global.css`파일 생성

```CSS
html {
  background-color: lavenderblush;
}
```

> css파일 배치는 임의적

## `gatsby-browser.js` 파일에 스타일 적용

1. `gatsby-browser.js`파일 만들기

```shell
cd ../..
touch gatsby-browser.js
```

프로젝트 구조

```TEXT
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

`gatsby-browser.js`파일은 갯츠비가 찾는 특수한 파일중 하나임
[참고](https://www.gatsbyjs.com/docs/browser-apis/)

2. `gatsby-browser.js` 파일에 스타일 추가

```JS
import "./src/styles/global.css"
// or:
// require('./src/styles/global.css')
```

> CommonJS(require)와 ES Module(import) 둘 다 사용가능.
> import가 좋지만, Node.js 환경 내에서만 실행되는 파일은 require 사용

3. dev 서버 실행

```shell
gatsby develop
```

![](/images/gatsby-styling/gatsby-styling_061441.png)

> 이 방법은 gastby-browser.js를 이용해 직접 css 파일을 가져옵니다.
> 가장 좋은 css 활용 방법은 컴포넌트 지향 방식입니다.

## 컴포넌트 스코프 CSS 활용

구성요소 범위 CSS 활용법을 알아보자.

### CSS 모듈

`CSS 모듈` 클래스 이름과 애니메이션 이름이 로컬로 범위가 지정되는 `CSS FILE`이다.

> CSS 모듈은 더욱 안전하며, 로컬 범위라 인기가 많다.
> 고유한 클래스와 애니메이션을 자동으로 생성하므로, 선택자 이름 충돌 걱정이 없다.
> React와 Gastby 사용 시 적극 권장!


### CSS Module로 새 페이지 만들기

