---
title: 안드로이드 에뮬레이터 IP 확인하기
description: >-
  결과부터 얘기하자면 android-studio 를 통해서 만든 에뮬레이터 IP는 10.0.2.15 이다. 10.0.2.15 라는 IP는
  virtualbox 전용 호스트 네트워크 연결 방식에서 사용되는 IP 이다. 아마도 이와 동일하거나 유사한…
date: '2017-12-14T21:37:23.794Z'
categories:
  - android
tags:
  - android
  - emulator
  - ip
last_modified_at: 2017-12-14T21:37:23.794Z
slug: >-
  /@moony211/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%97%90%EB%AE%AC%EB%A0%88%EC%9D%B4%ED%84%B0-ip-%ED%99%95%EC%9D%B8%ED%95%98%EA%B8%B0-ef8036005d00
---

결과부터 얘기하자면 android-studio 를 통해서 만든 에뮬레이터 IP는 10.0.2.15 이다. 10.0.2.15 라는 IP는 virtualbox 전용 호스트 네트워크 연결 방식에서 사용되는 IP 이다. 아마도 이와 동일하거나 유사한 방식으로 에뮬레이터와 host 컴퓨터 간에 연결이 되는 것으로 예측해본다.

그러나, 반드시 에뮬레이터가 android-studio 이란 법은 없다. 지난 번 포스팅 한 글에서도 에뮬레이터를 virtualbox에서 android-x86 배포판을 이용해서 만들었다. 네트워크 연결 방식은 바뀔 수 있고, IP도 바뀔 수도 있는 것이다. 에뮬레이터가 아닌 실제 장치도 동일한 방법으로 확인할 수 있을 것이다.

에뮬레이터도 결국 linux 기반의 장치이다. 그러므로, ifconfig 명령만 사용할 수 있다면 간단히 확인해볼 수 있다. 그러면, 이 linux 명령을 어떻게 실행해볼 것인가. 바로 adb shell 명령을 이용하면 된다.

android SDK 로부터 platform-tools 라는 디렉토리를 찾아 들어가면 adb 라는 프로그램을 발견할 수 있다. 에뮬레이터가 이미 연결이 되어 있는 상태라면 adb shell 이라고 입력한다. 오호, 에뮬레이터의 shell 모드로 들어갔다.

여기서부터는 linux 명령을 내려주면 된다.

```bash
ifconfig
```

IP 외에도 유용한 정보들을 빼내 볼 수 있을 것 같다.