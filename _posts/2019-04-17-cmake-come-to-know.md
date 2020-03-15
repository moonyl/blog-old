---
title: CMake 설치하기를 보면서 새롭게 알게된 내용
description: >-
  cmake를 사용하기 전에 어떻게 설치할 수 있을까를 설명한 내용이다. cmake 자체에 대한 내용은 없지만, 새로운 스킬을 익힐 수
  있었다.
date: '2019-04-17T17:47:34.325Z'
categories:
  - cmake
tags:
  - cmake
  - install
  - wget
  - .local
  - pip
last_modified_at: 2019-04-17T17:47:34.325Z
slug: >-
  /@moony211/cmake-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0%EB%A5%BC-%EB%B3%B4%EB%A9%B4%EC%84%9C-%EC%83%88%EB%A1%AD%EA%B2%8C-%EC%95%8C%EA%B2%8C%EB%90%9C-%EB%82%B4%EC%9A%A9-1aa6cb81bde2
---

[**Installing CMake · Modern CMake**  
_If you have a built in copy of CMake, it isn't special or customized for your system. You can easily install a new one…_cliutils.gitlab.io](https://cliutils.gitlab.io/modern-cmake/chapters/intro/installing.html "https://cliutils.gitlab.io/modern-cmake/chapters/intro/installing.html")[](https://cliutils.gitlab.io/modern-cmake/chapters/intro/installing.html)

cmake를 사용하기 전에 어떻게 설치할 수 있을까를 설명한 내용이다. cmake 자체에 대한 내용은 없지만, 새로운 스킬을 익힐 수 있었다.

먼저, wget 의 한 가지 유용한 개념이다. 내 경우에는 보통 wget은 웹서버나 ftp서버로부터 파일을 다운로드 받을 때 주로 사용했다. 그러나, -O-를 사용하면 표준출력으로 보낼 수 있고, 이것을 다시 파이프를 통해 다른 프로그램의 입력으로 보낼 수 있다. 이 방법을 파악하면, 다음 명령이 무슨 의도인지 명확해진다.

```bash
wget -qO- "https://cmake.org/files/v3.14/cmake-3.14.1-Linux-x86_64.tar.gz"
   | tar --strip-components=1 -xz -C ~/.local
```

같은 명령에서, tar 에 strip-components 라는 자주 볼 수 없었던 옵션을 발견했다. 이것을 이용하면, 압축파일의 디렉토리를 한 겹 더 벗겨서 해제할 수 있다. cmake-3.14.1-Linux-x86\_64.tar.gz 을 실제로 받아서 tar로 풀어보자. 확장자만 제외한 cmake-3.14.1-Linux-x86\_64 라는 디렉토리 내에 파일들을 풀어낸다. strip-components 를 1로 지정하게 되면 한 겹 더 벗겨서 풀어내어 cmake-3.14.1-Linux-x86\_64 안에 있던 bin, doc, man, share, 디렉토리를 바로 내놓는다.

왜 .local이라는 디렉토리에 압축을 해제했을까. 리눅스에서는 “.”으로 시작하는 디렉토리는 숨긴다는 의미이다. 이런 형태로 환경 설정이나 상태 저장용으로 많이 사용하는 것 같다. “.git” 과 같은 디렉토리로 말이다. “~/.local”에 파일 시스템 계층구조 표준(Filesystem Hierachy Standard, FHS)과 같은 형태로 입력해주니, 다음 번 로그인 시에, 이 디렉토리를 자동으로 path 환경 변수에 추가한다. /usr/bin이나 /usr/local/bin 같은 경우는 이 시스템을 사용하는 모슨 사용자에게 노출을 시켰다. 이것은 현재 사용자에게만 적용할 수 있는 효과를 얻을 수 있겠다.

pip를 이용해서 cmake를 설치할 수도 있는 것 같다. 개인적으로는 python을 거의 사용하고 있지 않아 pip 마저도 설치가 되어 있지 않은 관계로 실제로 시도해보지는 않았다.

[**Lmod: A New Environment Module System - Lmod 8.0.5 documentation**  
_Lmod is a Lua based module system that easily handles the MODULEPATH Hierarchical problem. Environment Modules provide…_lmod.readthedocs.io](https://lmod.readthedocs.io/en/latest/ "https://lmod.readthedocs.io/en/latest/")[](https://lmod.readthedocs.io/en/latest/)

Lmod는 Lua를 기반으로 한 모듈 시스템이라고 설명이 되어 있다. 역시 Lua는 아직은 파악하지 않은, 관심이 미치지 않은 스크립트 언어이므로, 더 깊은 조사는 이번엔 넘어간다.