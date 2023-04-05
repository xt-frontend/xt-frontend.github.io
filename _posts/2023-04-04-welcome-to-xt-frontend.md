---
layout: post
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다."
author: kangdaecheol
categories: [Blog Usage]
image: assets/images/12.jpg
featured: true
hidden: false
beforetoc: "beforetoc (목차 전 노출되는 글)"
toc: true
rating: 4.5
---

안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다. (본문)

> 안녕하세요! 기본적인 블로그 사용법에 대해 알려드리겠습니다.

```html
---
layout: post <!-- 게시 구분 "post로 설정" -->
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다." <!-- 본문 타이틀 -->
author: kangdaecheol <!-- 작성자 이름. _config.yml에 등록된 작성자 변수명과 동일하게 설정 -->
categories: [Blog Usage] <!-- 본문의 카테고리 구분. 본문 하단에 생성됨 -->
tags: [red, yellow] <!-- 본문의 태그. 본문 하단에 생성됨 -->
image: assets/images/11.jpg <!-- 필수값. 게시글에 노출되는 기본 이미지 및 썸네일 이미지 -->
toc: true <!-- true인 경우 본문 대타이틀 기준으로 본문 및 사이드바에 목차 생성. 클릭시 anchor이동 -->
beforetoc: "Test" <!-- 본문에서 목차 이전에 노출시킬 텍스트 -->
description: "Test" <!-- meta 태그 description -->
rating: 4.5 <!-- 별점 기능 (맛집 리뷰글 작성 시 유용할듯) -->
---
```

## CSS

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

## JS

```js
// alertbar later
$(document).scroll(function () {
  var y = $(this).scrollTop();
  if (y > 280) {
    $(".alertbar").fadeIn();
  } else {
    $(".alertbar").fadeOut();
  }
});
```

## Python

```python
print("Hello World")
```

## Ruby

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

## C

```c
printf("Hello World");
```
