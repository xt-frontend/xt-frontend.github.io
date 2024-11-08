---
layout: post
title: "React Project 공통정의"
author: kimgyutae
categories: [Convention]
tags: [React, Convention]
image: assets/images/logo-xt-lg.png
toc: true
---

## 패키지 구조

api

- api주소를 그대로 따름

assets

- fonts
- images

components

- common
- ui
- 카테고리별 정리

hooks

- 카테고리별 정리

pages // 리엑트, 리엑트 네이티브로 구분

state

types

utils

## 패키지별 정의

### 1) api

서버 API 연결

api에서 제공하는 패키지 구조를 그대로 사용하여 동일한 api가 추가 되지 않도록함

ex)

{{baseUrl}}/api/mb/cart/add // postman

api/mb/cart/add.ts // react project

```javascript
export const addCartItem = async (params: AddProductInCartParams) => {
  return axios.post(`/api/mb/cart/add`, params);
};
```

### 2) assets

폰트, 이미지등 리소스 모음

### 3) components

common ← 공통 컴포넌트

ui ← gnb, lnb sticky등 UI 컴포넌트

카테고리별 컴포넌트 ← 카테고리별로 정의되는 컴포넌트 정의, page나 screens에 포함시키지 않는 이유는 next.js나 nuxt.js의 자동 라우트 생성 기능으로 해당 패키지 하위에 위치하기 어려움

### 4) hooks

커스텀훅정의

주로 비동기 통신의 재활용에 사용됨 (feat. react query)

명명규칙은 앞에 use를 붙여 명명함

```javascript
const useGate = () => {
  const { mutate, isLoading, isError, error, isSuccess } = useMutation(gate);
  const [response, setResponse] = useState();

  const setRequest = (params: GateRequest) => {
    mutate(params, {
      onSuccess: (res) => {
        console.log(res.data);
      },
    });
  };

  return [response, setRequest];
};

export default useGate;
```

### 5) pages

웹페이지 또는 앱스크린별 페이지 정의

되도록 설계서에 정의된 path를 기반으로 작성 권유 (찾기가 쉬움)

### 6) state or store

전역상태관리정의

vue나 nuxt의 경우 store로 정의해도 무관

1뎁스 카테고리별로 파일생성을 권장

ex) 고객센터

state/customer.ts

react의 경우 atom을 사용하여 정의 하며,

명명규칙은 대문자로 시작하고, 뒤에 State를 붙여사용

```javascript
export const TabClickState =
  atom <
  boolean >
  {
    key: "TabClickState",
    default: false,
  };
```

### 7) types

타입스크립트의 타입을 정의

1뎁스 카테고리별로 파일생성을 권장

ex) 고객센터

types/customer.ts

명명규칙은 대문자로 시작하고, 뒤에 Type을 붙여 사용

```javascript
export interface ListRequestType {
  needDate: string;
  centerCode: string;
  upjang: string;
  orgUpjang: string;
  denyItemCodeList: [];
  evtItemCodeList: [];
}
```

### 8) utils

util함수나 common함수를 기능에 맞춰 파일을 생성하여 정의

## style의 작성

### 1) 컴포넌트별 Style

StyleSheet.create로 style을 정의

개별 확장은 가능하나 되도록 StyleSheet.create 내부에 작성하여 정리

```javascript
const Component = () => {
  return (
    <div>
      <p style={[styles.bottomWrap, { color: "red" }]}></p>
    </div>
  );
};
```

```javascript
const styles = StyleSheet.create({
  bottomWrap: {
    position: "absolute",
    bottom: 0,
    width: "100%",
    alignItems: "center",
  },
});
```

### 2) Theme 적용 Style

정의된 theme style을 반영 하는 방식

```javascript
const Component = () => {
  const theme = useTheme();
  const styles = makeStyles(theme);

  return (
    <div>
      <p style={[styles.bottomWrap, { color: "red" }]}></p>
    </div>
  );
};
```

```javascript
const makeStyles = (theme: any) =>
  StyleSheet.create({
    bottomWrap: {
      backgroundColor: theme.colors.primary,
      alignItems: "center",
    },
  });
```
