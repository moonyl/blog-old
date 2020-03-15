---
title: cmake로 ffmpeg 라이브러리 가져오기
description: >-
  윈도우에서 ffmpeg 라이브러리를 가져와서 링크시키는 작업을 수작업으로 계속해왔다. 그러다보니 ffmpeg 라이브러리 자체도 함께
  svn이나 git에 함께 포함시켜서 관리하고 공유하는 형태가 되어 버렸다. 물론 한번 작업해 놓으면 그 뒤에 불편한…
date: '2020-02-28T23:37:07.873Z'
categories:
  - cmake
tags:
  - cmake
  - externalProject 
  - ffmpeg
last_modified_at: 2020-02-28T23:37:07.873Z
slug: >-
  /@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0-ce659b02b829
---

윈도우에서 ffmpeg 라이브러리를 가져와서 링크시키는 작업을 수작업으로 계속해왔다. 그러다보니 ffmpeg 라이브러리 자체도 함께 svn이나 git에 함께 포함시켜서 관리하고 공유하는 형태가 되어 버렸다. 물론 한번 작업해 놓으면 그 뒤에 불편한 점은 없지만 버전 관리 서버 입장에서는 공간 낭비가 될 수 있기도 하고, 왠지 정석적인 방법이 아닌 것 같다.

cmake에서는 외부의 라이브러리를 쉽게 다운로드 및 압축 해제를 하고, 필요에 따라서는 빌드와 설치를 할 수 있도록 ExternalProject\_Add 라는 함수를 제공한다.

[**ExternalProject - CMake 3.17.0-rc1 Documentation**  
_ExternalProject\_Add The ExternalProject\_Add() function creates a custom target to drive download, update/patch…_cmake.org](https://cmake.org/cmake/help/latest/module/ExternalProject.html "https://cmake.org/cmake/help/latest/module/ExternalProject.html")[](https://cmake.org/cmake/help/latest/module/ExternalProject.html)

설명이 그리 쉽지는 않기 때문에 내용을 참조해서 실전으로 해보면서 관련 파라메터를 정리하는 것이 더 머리속에 남을 것 같다.

우선, 윈도우의 ffmpeg 라이브러리와 실행 파일은 제라노 — 사람 이름인가? — 라는 사이트에서 지속적으로 빌드해서 제공하고 있다. 그래서 그냥 다운로드 받아서 프로그램을 실행하던지, 라이브러리를 링크하던지 하면 된다.

[**FFmpeg Builds**  
_FFmpeg is the leading multimedia framework to decode, encode, transcode, mux, demux, stream, filter and play. All…_ffmpeg.zeranoe.com](https://ffmpeg.zeranoe.com/builds/ "https://ffmpeg.zeranoe.com/builds/")[](https://ffmpeg.zeranoe.com/builds/)

이 작업을 직접적으로 다운로드 받아서, 압축을 해제해서 사용해도 된다. 그렇지만, ExternalProject\_Add 함수를 사용하면, 이 작업을 자동으로 실행할 수 있다.

```cmake
include(ExternalProject)
```

우선적으로 이 함수를 사용하려면, ExternalProject라는 모듈을 포함시켜야 한다.

다운로드 받을 위치를 URL 이라는 설정값으로 지정해준다.

```cmake
ExternalProject_Add(ffmpeg  
        URL [https://ffmpeg.zeranoe.com/builds/win64/dev/ffmpeg-4.2.2-win64-dev.zip](https://ffmpeg.zeranoe.com/builds/win64/dev/ffmpeg-4.2.2-win64-dev.zip)  
)
```

지금까지 설정으로, cmake는 zip으로 묶인 파일을 다운로드 받아서 압축해제를 할 것이다. 그러나, 이 함수는 몇 가지 단계를 자동적으로 실행시킨다. 몇 가지 추가적인 단계가 더 있지만, configure → build → install 이라는 작업은 기본적으로 실행되는 것으로 보인다. 그래서 앞에서의 함수 호출은 에러를 발생시킨다.

일단은 다운로드 및 해제만 시켜보자. 이를 위해서는 세 가지의 단계에 대한 명령을 하지 않도록 해주면 된다.

```cmake
ExternalProject_Add(ffmpeg  
        URL https://ffmpeg.zeranoe.com/builds/win64/dev/ffmpeg-4.2.2-win64-dev.zip  
        CONFIGURE_COMMAND ""  
        BUILD_COMMAND ""  
        INSTALL_COMMAND ""  
)
```

이제 cmake를 configure 및 build를 실행해보자.

```bash
cmake -Bbuild  
cd build  
cmake --build .
```

빌드 작업이 진행되면서, 다운로드와 압축 해제가 실행되는 과정을 볼 수 있다.

```
.NET Framework용 Microsoft (R) Build Engine 버전 16.4.0+e901037fe  
Copyright (C) Microsoft Corporation. All rights reserved.

...  
  Performing download step (download, verify and extract) for 'ffmpeg'  
  -- Downloading...  
...  
  -- [download 0% complete]  
  -- [download 1% complete]  
...  
  -- [download 90% complete]  
  -- [download 100% complete]  
  -- Downloading... done  
  -- extracting...  
...  
  -- extracting... done  
  No update step for 'ffmpeg'  
  No patch step for 'ffmpeg'  
  No configure step for 'ffmpeg'  
  No build step for 'ffmpeg'  
  No install step for 'ffmpeg'  
  Completed 'ffmpeg'  
...
```

build/ffmpeg-prefix/src 위치에 zip파일이 다운로드 된 것을 확인할 수 있다. 그리고, build/ffmpeg-prefix/src/ffmpeg에 압축파일이 해제되어 설치된 것을 확인할 수 있다.

[소스 코드](https://github.com/moonyl/link-ffmpeg-with-cmake/archive/v0.1.zip)를 올려두었다. 앞으로 기능 추가 및 개선 작업을 위해서 github에 관리한다.

[**moonyl/link-ffmpeg-with-cmake**  
_cmake를 이용해서 ffmpeg 라이브러리를 자동으로 다운로드하고 링크한다. Contribute to moonyl/link-ffmpeg-with-cmake development by creating an…_github.com](https://github.com/moonyl/link-ffmpeg-with-cmake "https://github.com/moonyl/link-ffmpeg-with-cmake")[](https://github.com/moonyl/link-ffmpeg-with-cmake)

이 글 다음에 이어지는 글도 작성했다.

[**cmake로 ffmpeg 라이브러리 설치하기**  
_앞 선 글에서 제라노 사이트에서 ffmpeg 패키지를 다운로드할 수 있는 방법을 정리했다. 여기에서 불편한 점이 있다. cmake의 build 명령을 실행해야 다운로드가 실행된다는 점, 다운로드 받은 위치가…_medium.com](https://medium.com/@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-d88214f284c3 "https://medium.com/@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-d88214f284c3")[](https://medium.com/@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-d88214f284c3)