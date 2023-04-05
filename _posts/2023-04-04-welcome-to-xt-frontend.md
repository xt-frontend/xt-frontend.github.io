---
layout: post
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다."
author: kangdaecheol
categories: [Blog Usage]
image: assets/images/12.jpg
featured: true
hidden: false
beforetoc: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다."
toc: true
---

안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다.

> 안녕하세요! 기본적인 블로그 사용법에 대해 알려드리겠습니다.

```html
---
layout: post
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다."
author: kangdaecheol
categories: [Blog Usage]
tags: [red, yellow]
image: assets/images/11.jpg
description: "My review of Inception movie. Actors, directing and more."
rating: 4.5
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
