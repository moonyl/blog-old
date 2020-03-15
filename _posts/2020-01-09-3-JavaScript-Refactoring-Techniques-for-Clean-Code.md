---
title: “3 JavaScript Refactoring Techniques for Clean Code”을 읽고…
description: 기본적이지만 가독성을 높이기 위한 리팩토링 3가지 방법을 정리해서 머릿 속에 남겨둬야 겠다는 생각으로 기록해둔다.
date: '2020-01-09T00:24:19.285Z'
categories:
  - theory
tags:
  - javascript
  - position
  - refactoring
  - clean-code
last_modified_at: 2020-01-09T00:24:19.285Z
slug: >-
  /@moony211/3-javascript-refactoring-techniques-for-clean-code-%EC%9D%84-%EC%9D%BD%EA%B3%A0-335b022a5238
---

기본적이지만 가독성을 높이기 위한 리팩토링 3가지 방법을 정리해서 머릿 속에 남겨둬야 겠다는 생각으로 기록해둔다.

[**3 JavaScript Refactoring Techniques for Clean Code**  
_Some simple tips to refactor your code and make it clearer in any language._levelup.gitconnected.com](https://levelup.gitconnected.com/3-javascript-refactoring-techniques-for-clean-code-c356be1abbcb "https://levelup.gitconnected.com/3-javascript-refactoring-techniques-for-clean-code-c356be1abbcb")[](https://levelup.gitconnected.com/3-javascript-refactoring-techniques-for-clean-code-c356be1abbcb)

먼저, 마틴 파울러님이 하신 말씀을 마음에 새겨둔다.

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

컴퓨터가 이해할 수 있는 코드는 누구나 만들 수 있다. 좋은 프로그래머는 사람이 이해할 수 있는 코드를 만든다.

자존심과 자긍심이 강한 개발자라면 리팩토링을 하게 될 충분한 동기를 일으키는 말씀이라 생각된다.

3가지 방법은 간단한다.

*   함수로 뽑아내기
*   파라메터를 하나로 묶기
*   함수들을 묶어서 클래스로 만들기

참조한 글의 예제를 직접 작성해 보면서 곱씹어봤다.

## 함수로 뽑아내기

가독성을 높이려면 함수, 변수, 클래스등에 이름을 부여한대로 내부 기능을 구현해야 한다고 생각한다. 3가지 방법 중 첫번째 부분은 특히, 이러한 관점에서 중요한 방법이다.

## 파라메터를 하나로 묶기

모든 파라메터를 하나로 만든다는 의미가 아니라, 여러 함수들에서 공통적으로 사용되는 파라메터는 서로 밀접한 관계가 있다고 볼 수 있다. 이것을 하나로 묶은 구조체로 관리함으로 추상화 수준을 높이고, 생각을 단순화할 수 있는 장점을 발휘할 수 있다고 본다.

## 함수들을 묶어서 클래스로 만들기

파라메터를 묶는 것과 유사하게, 공통된 변수나 자료 형태를 이용하는 함수들을 하나의 클래스로 묶으면, 가독성 뿐만 아니라 모듈화하기도 좋기 때문에 재사용성 또한 높일 수 있다고 생각한다.

리팩토링을 하기 위한 3가지 기본 방침을 잘 기억해두고, 언제든지 적용할 수 있는 것이 중요하다.