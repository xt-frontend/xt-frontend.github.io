---
layout: post
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다."
author: kangdaecheol
categories: [Blog Usage]
tags: [블로그 사용 방법]
image: assets/images/logo-xt-lg.png
beforetoc: "본 게시글은 기본적인 github 블로그 사용 방법에 대해 알려드리는 글입니다."
toc: true
rating: 5
---

## 블로그 작성을 위한 기본 설정

1. <a href="https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home" target="_blank">github 계정을 생성</a>합니다.
2. 계정 생성 후 git 초대까지 완료 되었으면, <a href="https://github.com/xt-frontend/xt-frontend.github.io" target="_blank">https://github.com/xt-frontend/xt-frontend.github.io</a>에 접속 후 sourcetree를 이용해 clone을 받습니다.
3. clone 받은 프로젝트 폴더를 vscode로 열고 작업합니다.
4. push시 에러가 발생하면 <a href="https://ssimplay.tistory.com/787" target="_blank">해당 글을 참고</a>하여 sourcetree와 github 계정의 토큰을 연동합니다.

## 게시글 작성법

기본적인 글 작성법입니다.<br />
작성할 본문 파일은 \_posts 폴더에 생성합니다. <br />
파일 생성 시 주의할 점은 정해진 양식에 맞춰 파일명을 작성해야 합니다.

YYYY-MM-DD-title.md
<br />
ex)2023-04-04-welcome-to-xt-frontend.md

### 1) 게시글 기본 설정

파일을 생성했다면 최상단에 아래 코드와 같이 작성해줍니다.
아래 코드는 타이틀을 비롯한 게시글의 기본 설정값으로, 필수값을 제외하고 필요한 값만 작성해주면 됩니다.

```html
---
layout: post <!-- 필수값. 게시 구분. page, post 중 "post로 설정" -->
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다." <!-- 필수값. 본문 타이틀 -->
description: "Test" <!-- 필수값. meta 태그 description -->
author: kangdaecheol <!-- 필수값. 작성자 이름. _config.yml에 등록된 작성자 변수명과 동일하게 설정 -->
categories: [Blog Usage] <!-- 선택값. 본문의 카테고리 구분. 본문 하단에 생성됨 -->
tags: [red, yellow] <!-- 선택값. 본문의 태그. 본문 하단에 생성됨 -->
image: assets/images/11.jpg <!-- 필수값. 게시글에 노출되는 기본 이미지 및 썸네일 이미지 -->
featured: true <!-- 선택값. 홈의 상단 featured에 노출 시킬지 여부. -->
hidden: false  <!-- 선택값. featured: true인 경우 홈의 하단 All Posts에 게시글을 노출 시킬지 여부 -->
toc: true <!-- 선택값. true인 경우 본문 타이틀 기준으로 본문 및 사이드바에 목차 생성. 클릭시 해당 타이틀 위치로 anchor이동 -->
beforetoc: "Test" <!-- 선택값. toc: true일 경우 본문에서 목차 이전에 노출시킬 텍스트 -->
rating: 4.5 <!-- 선택값. 별점 기능. 0부터 5까지 0.5단위로 작성 -->
---
```

### 2) 본문 내용 작성

본문 내용은 앞서 작성한 게시글 기본 설정 바로 아래에 작성합니다.<br />
기본적으로 태그를 감싸서 작성할 필요는 없지만, 이미지나 특정 클래스에 css를 주고 싶은 경우
태그를 감싸서 작성할 수 있습니다.

```html
Example
<img src="/assets/images/12.jpg" alt="image" />
<p class="test">태그 안에 감싸서 작성 가능</p>
```

### 3) 마크다운(Markdown) 문법

태그를 감싸지 않고 h2~h6 태그나 리스트, 인용문, 텍스트 강조, 코드 미러 등 다양한 마크업 기능을 사용할 수도 있습니다.

이러한 문법을 `마크다운(Markdown)문법`이라고 합니다.

#### - h2~h6 태그

```html
## 타이틀명
<!-- 본문에서 h2태그로 변환됩니다. toc: true일 경우 1Depth 메뉴가 됩니다. -->

### 타이틀명
<!-- 본문에서 h3태그로 변환됩니다. toc: true이고, h2태그가 선언되어 있는 경우 앞서 선언한 h2태그의 2Depth 메뉴가 됩니다.  -->

#### 타이틀명
<!-- 본문에서 h4태그로 변환됩니다. toc: true이고, h2,h3태그가 선언되어 있는 경우 앞서 선언한 h2태그의 3Depth 메뉴가 됩니다.  -->

##### 타이틀명
<!-- 본문에서 h5태그로 변환됩니다. toc: true이고, h2,h3,h4태그가 선언되어 있는 경우 앞서 선언한 h2태그의 4Depth 메뉴가 됩니다.  -->

###### 타이틀명
<!-- 본문에서 h6태그로 변환됩니다. toc: true이고, h2,h3,h4,h5태그가 선언되어 있는 경우 앞서 선언한 h2태그의 5Depth 메뉴가 됩니다.  -->
```

#### - 리스트

리스트는 크게 도트형 리스트, 숫자형 리스트로 나뉩니다.

```html
<!-- 도트형 리스트 

- 도트형 리스트1
- 도트형 리스트2
- 도트형 리스트3

-->

<!-- 숫자형 리스트

1. 숫자형 리스트1 
2. 숫자형 리스트2 
3. 숫자형 리스트3

-->
```

Result

- 도트형 리스트1
- 도트형 리스트2
- 도트형 리스트3

1. 숫자형 리스트1
2. 숫자형 리스트2
3. 숫자형 리스트3

#### - 인용문

```html
<!-- 인용문

> 안녕하세요! 기본적인 블로그 사용 방법에 대해 알려드리겠습니다. - XT 프론트엔드 개발실 강대철

-->
```

Result

> 안녕하세요! 기본적인 블로그 사용 방법에 대해 알려드리겠습니다. - XT 프론트엔드 개발실 강대철

#### - 인라인 코드 블럭

```html
<!-- 인라인 코드블럭

`인라인 코드 블럭`

이 텍스트는 `인라인 코드 블럭`입니다.

-->
```

Result

`인라인 코드 블럭`

이 텍스트는 `인라인 코드 블럭`입니다.

#### - 텍스트 꾸미기

```html
<!-- 이텔릭체 , 볼드체, 취소선, 밑줄

`강조 텍스트`

기울여진 텍스트입니다*
***굵고 기울여진 텍스트입니다***

~~취소된 텍스트입니다~~

<u>밑줄 있는 텍스트입니다</u>

-->
```

Result

_이탤릭체_

_이탤릭체_

**볼드**

~~취소선~~

<U>밑줄</U>

#### - 링크

```html
<!-- 이텔릭체 , 볼드체, 취소선, 밑줄

[link keyword](URL) 

-->
```

Result

[깃헙 블로그](https://github.blog/)

#### - 코드 미러

HTML

````html
<!-- 
```html
<div class="container">
  <p class="typography">텍스트</p>
</div>
```
-->

<!-- Result -->
<div class="container">
  <p class="typography">텍스트</p>
</div>
````

CSS

````css
/*
```css
.highlight .c {
  color: #999988;
  font-style: italic;
}
.highlight .err {
  color: #a61717;
  background-color: #e3d2d2;
}
```
*/

/* Result */
.highlight .c {
  color: #999988;
  font-style: italic;
}
.highlight .err {
  color: #a61717;
  background-color: #e3d2d2;
}
````

JS

````js
// ```js
// $(document).scroll(function () {
//   var y = $(this).scrollTop();
//   if (y > 280) {
//     $(".alertbar").fadeIn();
//   } else {
//     $(".alertbar").fadeOut();
//   }
// });
// ```

// Result
$(document).scroll(function () {
  var y = $(this).scrollTop();
  if (y > 280) {
    $(".alertbar").fadeIn();
  } else {
    $(".alertbar").fadeOut();
  }
});
````

Python

````python
# ```python
# print("Hello World")
# ```

# Result
print("Hello World")
````

Ruby

````ruby
# ```ruby
# require 'redcarpet'
# markdown = Redcarpet.new("Hello World!")
# puts markdown.to_html
# ```

# Result
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
````

C

````c
// ```c
// printf("Hello World");
// ```

printf("Hello World");
````

더 다양한 `마크다운(Markdown)문법`을 확인하고 싶으면 <a href="https://gist.github.com/ihoneymon/652be052a0727ad59601" target="_blank">해당 링크</a>에서 확인하시면 됩니다.

## 마무리

지금까지 github 블로그 사용 방법에 대해 알아보았습니다. <br />
글로 이해가 잘 안되실 경우, \_posts/2023-04-04-welcome-to-xt-frontend.md 파일을<br /> 참고하시면 이해에 도움이 될 것 같습니다.<br />감사합니다.
