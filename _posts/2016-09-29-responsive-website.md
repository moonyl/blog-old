---
title: "반응형 웹사이트"

categories:
  - web-devel
tags:
  - responsive
  - media
  - max-width
  - min-width
last_modified_at: 2016-09-29T04:40:00-05:00
---
웹문서는 하나이고 CSS 를 여러 개를 사용해서 핸드폰, 태블릿, PC 상에서 모두 가독성이 좋도록 구성해주는 방식을 말한다. 뭐 내용을 거창한 것 같으나, 지정하는 방법은 간단하다.

```css
@media screen and (max-width:767px) {
/* 핸드폰 용 CSS */
}

@media screen and (max-width:959px) and (min-width:768px) {
/* 태블릿 용 CSS */
}

@media screen and (min-width:960px) {
/* PC 용 CSS */
}
```

배치의 문제가 더 어려운 작업일 듯 하다. PC 화면 상에서는 주로 메뉴가 상단에 구성이 되어 있는 반면, 태블릿은 좌측에 보이고, 나타났다가 사라졌다가 하는 기능도 들어가는 경우도 많다. 핸드폰 용은 메뉴와 컨텐츠가 아예 따로 구성이 되어 있는 경우가 많다. 이러한 방식의 차이도 반응형 웹사이트 구축 방법으로 구성이 가능한지 더 공부해봐야겠다.
