---
layout: post
title: "[Emotion.JS👩‍🎤]"
author: ohaneul
categories: [GSAP]
tags: [GSAP]
image: assets/images/emotion/emotion-banner.png
toc: true
---

# :8ball: EmotionJS

`Emotion.JS`은 이름에서 알 수 있듯, javascript에서 CSS를 작성 할 수 있는 스타일입니다.

초기 세팅을 해주면 className이 초기 설정값에 맞게 자동 부여가 됩니다.

## :8ball: 용량&성능

**https://bundlehobia.com/**를 참고해 최신 라이브러리 사이즈를 확인해봅시다.
라이브러리 용량은 비슷해보이지만 `@emotion/react`와 `styled-components`를 놓고 비교해 봤을 땐 1.5배 정도 차이가 납니다.

<img src="/assets/images/emotion/emotion-img01.png" alt="emotion" />

<img src="/assets/images/emotion/emotion-img02.png" alt="emotion" />

<img src="/assets/images/emotion/emotion-img03.png" alt="emotion" />

## :8ball: 초기설정

만약 styled-component로 사용하고 싶다면 `@emotion/styled`를 설치하시면 됩니다.

```
$ npm install @emotion/react

$ npm install @emotion/styled
```

아래와 같이 className을 커스터마이징 하려면 `@emotion/babel-plugin` 을 설치하면 됩니다.

```
$ npm install @emotion/babel-plugin
```

<img src="/assets/images/emotion/emotion-img04.png" alt="emotion" />

### 📂 `/.babelrc`

`labelFormat` 에서 설정값을 지정해주면 세팅값에 따라서 `className`이 지정됩니다.

```jsx
{
  "presets": ["next/babel", "@emotion/babel-preset-css-prop"],
  "plugins": [
    [
      "@emotion",
      {
        "autoLabel": "dev-only", //기본값
        "labelFormat": "[dirname]-[filename]-[local]"
      }
    ]
  ]
}
```

### 📂 /tsconfig.json

css props 설정

문자열 스타일을 전달하려면 @emotion/react에서 내보낸 css를 사용해야하며, 아래와 같이 태그된 [템플릿 리터럴](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 로 사용할 수 있습니다.

## 🎱 The css Prop

### 1. Object Styles

객체 스타일 작성법은 일반 CSS처럼 kebab-case로 css 속성을 작성하는 대신 camelCase로 작성합니다.
객체 스타일 작성법은 문자열 작성법과 달리 CSS 호출이 필요하지 않지 않습니다.

**With the css prop**

```jsx
render(
  <div
    css={{
      color: "darkorchid",
      backgroundColor: "lightgray",
    }}
  >
    This is darkorchid.
  </div>
);
```

**Child Selectors**

```jsx
render(
  <div
    css={{
      color: "darkorchid",
      "& .name": {
        color: "orange",
      },
    }}
  >
    This is darkorchid.
    <div className="name">This is orange</div>
  </div>
);
```

**Numbers**
css 속성값으로 숫자를 쓸 경우 px 단위를 생략할 수 있습니다.

```jsx
render(
  <div
    css={{
      padding: 8,
      zIndex: 200,
    }}
  >
    This has 8px of padding and a z-index of 200.
  </div>
);
```

**Arrays**
객체를 중복 해서 사용할 경우 배열에 담아줍니다.

```jsx
render(
  <div
    css={[
      { color: "darkorchid" },
      { backgroundColor: "hotpink" },
      { padding: 8 },
    ]}
  >
    This is darkorchid with a hotpink background and 8px of padding.
  </div>
);
```

### 2. With `css`

객체 스타일과 함께 css를 사용할 수도 있습니다.

```jsx
import { css } from "@emotion/react";

const hotpink = css({
  color: "hotpink",
});

render(
  <div>
    <p css={hotpink}>This is hotpink</p>
  </div>
);
```

## 🎱 조건부 스타일링 적용하기

### Emotion CSS 기본 사용법

`Emotion CSS`는 기본적으로 `styled-component`와 작성 법이 유사합니다.

변수 하나를 선언하고 그 안에 템플릿 리터럴을 이용해 `CSS` 내용을 변수에 넣어둡니다.

이제 해당 스타일을 적용할 부분에 `css={변수명}`을 작성해주면 됩니다.

```html
<div css="{container}">
  <h1 className="title">Emotion CSS</h1>
  <p className="desc">조건부 스타일링 적용하기</p>
</div>
```

```jsx
const contiainer = css`
  .title {
    font-size: 30px;
    color: pink;
    .desc {
      fot-size: 18px;
      color: gray;
    }
  }
`;
```

### 조건부 스타일링

조건에 따라 css를 다르게 적용 시키고 싶을땐

css를 담은 변수에 `props`로 값을 전달해주면 됩니다

```jsx
const normalContainer = css``; // 기존 방식
const ConditionalContainer = (props) => css``; //props 전달하는 방식
```

`괄호()` 안에 요소를 담아 전달합니다.

```jsx
<div css={CoditionalContainer({ tp })}></div>;

const ConditionalContainer = (props) => css`
  padding: 2rem;
  color: ${props.tp === 3
    ? "red"
    : props.nowTime === 2
    ? "blue"
    : props.nowTime === 1
    ? "green"
    : "black"};
`;
```

## EmotionJS 문제점

`Emotion CSS`는 CSS-in-작성 방식으로 인라인스타일로 직정 스타일을 적용 할 수 있습니다.

```jsx
<span
  css={css`
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    line-height: 1.5;
    word-break: break-all;
    white-space: no-wrap;
  `}
>
  {text}
</span>
```

위와 같이 작성한다면 직관적로 css 적용을 확인 할 수 있다는 장점이 있지만
많은 줄의 코드가 작성 될 경우 가독성을 해친다는 문제가 있습니다.

## 🎱 비슷한 듯 다른 Emotion 👩‍🎤 VS styled-copoment 💅

`emotion`은 `styled-comopnents` 작성법도 지원하는 것을 보고
`styled-compents`와 `emotion` 간에 어떤 차이점이 있을까라는 생각이 들어 정리해보았습니다.

1. TypeScript

- `styled-components`에서는 타입스크립트를 지원하지 않아 `@types/styled-components`를 추가로 설치 해야하지만 `emotion`의 경우 자체에서 타입스크립트를 지원해 별도로 설치 할 필요가 없습니다.

2. Server Side Rendering

- `@emotion`의 경우 서버측 렌더링은 `emotion 10`이상에서 기본적으로 동작합니다.
- 반면 `styled-compoents`같은 경우 [ServerStyleSheet](https://styled-components.com/docs/advanced#streaming-rendering)를 설정해야 합니다.
  - Next.js에서 제공하는 예시 참고링크 [styled-compoents](https://styled-components.com/docs/advanced#server-side-rendering) 공식문서

## 참고 문서

1. [CSS-in-JS](https://github.com/andreipfeiffer/css-in-js/blob/main/README.md#styled-components){:target="\_blank"}
2. [Emotion CSS](https://emotion.sh/docs/introduction){:target="\_blank"}
