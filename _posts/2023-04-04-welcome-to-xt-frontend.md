---
layout: post
title: "안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다."
author: kangdaecheol
categories: [Blog Usage]
image: assets/images/12.jpg
featured: true
hidden: false
---

안녕하세요. XT 프론트엔드 개발실 기술 블로그입니다.

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

#### JS

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

#### Python

```python
print("Hello World")
```

#### Ruby

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

#### C

```c
printf("Hello World");
```
