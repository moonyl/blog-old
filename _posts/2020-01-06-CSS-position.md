---
title: CSS position에 대한 고찰
description: >-
  나는 UI 경험이 그리 많지 않다. 그래서 웹앱을 개발하면서 UI관련 작업에서 고통을 좀 받겠지만, 특히 CSS의 배치 문제는 이해하기가
  쉽지 않다. position 이라는 특성값을 써보려고 할 때도 마찬가지였다.
date: '2020-01-06T14:24:21.888Z'
categories:
  - web-devel
tags:
  - css
  - position
  - static
  - relative
  - fixed
  - absolute
  - sticky
last_modified_at: 2020-01-06T14:24:21.888Z
slug: >-
  /@moony211/css-position%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0-d97a3928152f
---

나는 UI 경험이 그리 많지 않다. 그래서 웹앱을 개발하면서 UI관련 작업에서 고통을 좀 받겠지만, 특히 CSS의 배치 문제는 이해하기가 쉽지 않다. position 이라는 특성값을 써보려고 할 때도 마찬가지였다.

이제는 조금 이해했다 싶지만, 갈수록 기억력이 떨어지고 있으므로, 다음에 볼 때, 새로운 내용으로 보일 수 있으므로, 간략하게나마 기록을 남겨둬야 할 것 같다.

position은 원소(element)가 배치되는 방식을 지정하는 것이라 할 수 있다. 이 특성에 대한 값으로 사용할 수 있는 값은 5가지가 있다.

*   static
*   relative
*   fixed
*   absolute
*   sticky

static은 기본값이므로, 사용하지 않더라도 지정되는 값이다. 원소가 배치될 때 블록레벨 원소인 경우에는 다음 원소는 아래 방향으로, 인라인 원소인 경우에는 오른쪽 방향으로 겹치치 않으면서 나타나는 방식이다.

relative는 static에서 top, left, bottom, right를 적용해서 원소의 배치를 조정할 수 있는 방식이다. static 방식에서는 이 네 가지 값이 전혀 먹지 않는다.

fixed는 view port를 기준으로 네 가지 값을 적용할 수 있다. view port는 브라우저의 보이는 영역이라 생각하면 될 것 같다. fixed 방식에서 bottom과 right값을 0으로 지정한 경우를 생각해볼 수 있다. 문서의 양이 많아서 스크롤이 발생하는 경우더라도 이 원소는 view port에 고정되므로, 브라우저의 우측 하단에 고정되어 이동되지 않고 표시된다.

absolute는 네 가지 설정값이 부모 원소 기준으로 적용된다. 좀 특이하게도 부모가 static으로 지정된 경우에는 이 기준으로 되지 않고, 더 상위의 static이 아닌 부모로 기준이 바뀐다. 부모가 relative, fixed, sticky, absolute인 경우에는 이를 기준으로 배치가 된다.

sticky는 relative와 fixed가 혼합된 경우로 생각해볼 수 있다. sticky 방식에서 top을 0으로 설정한 경우를 생각해볼 수 있다. 스크롤이 발생하여, sticky 방식으로 지정된 원소가 view port의 top이 0인 위치까지는 relative와 동일하게 배치되다가 그 보다 더 아래로 이동하게 될 경우에는 fixed 방식처럼 위치가 고정되어 표시된다.

각 position 방식의 차이를 쉽게 파악할 수 있도록 codepen 에 간단히 예제를 작성했다.

position 특성값 예제

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="moonyl" data-slug-hash="JjoMMog" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="JjoMMog">
  <span>See the Pen <a href="https://codepen.io/moonyl/pen/JjoMMog">
  JjoMMog</a> by Sang-moon, Lee (<a href="https://codepen.io/moonyl">@moonyl</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

그리 어려운 개념은 아니지만, 익숙치 않기 때문에 처음에 이해하기가 쉽지 않았던 것 같다. 정리를 한번 해두면 잊어버리더라도 금방 파악이 될 것이라 기대한다.