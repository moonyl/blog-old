---
title: 'SOLID Programming: Single Responsibility Principle 을 읽고…'
description: >-
  소프트웨어를 설계하고 구현하는데, SOLID라는 말은 귀에 딱지가 앉을 정도로 많이 들어왔다. 그러나, 정작 그 용어 자체를 명확하게
  이해하지 못했을 뿐더러, 실제 개발 시에도 잘 적용하지 못했다. 오늘도 또 관련된 글을 읽고 말았다.
date: '2020-01-10T00:17:39.290Z'
categories:
  - essay
tags:
  - SOLID
  - SRP
  - single-responsibility
last_modified_at: 2020-01-10T00:17:39.290Z
slug: >-
  /@moony211/solid-programming-single-responsibility-principle-%EC%9D%84-%EC%9D%BD%EA%B3%A0-67561947ed2f
---

[**SOLID Programming (Part 1): Single Responsibility Principle**  
_SOLID principles are among the most valuable in Software Engineering. They allow to write code that is clean, scalable…_towardsdatascience.com](https://towardsdatascience.com/solid-programming-part-1-single-responsibility-principle-efca5e7c2a87 "https://towardsdatascience.com/solid-programming-part-1-single-responsibility-principle-efca5e7c2a87")[](https://towardsdatascience.com/solid-programming-part-1-single-responsibility-principle-efca5e7c2a87)

소프트웨어를 설계하고 구현하는데, SOLID라는 말은 귀에 딱지가 앉을 정도로 많이 들어왔다. 그러나, 정작 그 용어 자체를 명확하게 이해하지 못했을 뿐더러, 실제 개발 시에도 잘 적용하지 못했다. 오늘도 또 관련된 글을 읽고 말았다.

single responsibility, 단일 책임이라고 한글로 번역은 많이 되고 있는 것 같다. 로버트 마틴님은 책임을 “a reason to change” 라고 정의하셨는데, 그다지 잘 와닿지 않는다. 변경이 어디서 될 것이라고 예측하는 것도 머리 아프고, 새로운 스킬을 익히기도 바쁘다.

그나마, 리팩토링의 소중함은 체감하고 있는 중이라, 틈틈이 DRY를 기반으로 실천하고자 노력하는데, SRP(Single Responsibility Principle) 가 걸리는 건 왜일까.

참조한 글을 읽으면서, 한가지 시도할 방법이 떠올랐는데, 이것을 기록에 남기려고 한다.

> 기능을 한 개의 단문장으로 적어보고, 이름을 정한다. 이것을 함수로 만들어 분리한다.

기능이 작게 잘 분리된 함수나 클래스를 통해 구현을 하게 된다면, 글을 읽듯이 코드를 읽을 수 있고, 로직이 머릿 속에 잘 정리가 될 것이라 예상할 수 있다. 테스트하기도 좋아질테니, TDD 같은 개발방법을 사용하는 개발자라면, 이 원칙은 중요할 것이라 생각해본다. 언제쯤 멋있고, 자신있는 개발을 하게 되려나…