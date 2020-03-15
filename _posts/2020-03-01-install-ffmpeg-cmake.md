---
title: cmake로 ffmpeg 라이브러리 설치하기
description: >-
  앞 선 글에서 제라노  사이트에서 ffmpeg 패키지를 다운로드할 수 있는 방법을 정리했다. 여기에서 불편한 점이 있다. cmake의
  build 명령을 실행해야 다운로드가 실행된다는 점, 다운로드 받은 위치가 build하기 위해 지정한 디렉토리의…
date: '2020-03-01T14:08:27.415Z'
categories:
  - cmake
tags:
  - cmake
  - externalProject 
  - ffmpeg
last_modified_at: 2020-03-01T14:08:27.415Z
slug: >-
  /@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-d88214f284c3
---

[**cmake로 ffmpeg 라이브러리 가져오기**  
_윈도우에서 ffmpeg 라이브러리를 가져와서 링크시키는 작업을 수작업으로 계속해왔다. 그러다보니 ffmpeg 라이브러리 자체도 함께 svn이나 git에 함께 포함시켜서 관리하고 공유하는 형태가 되어 버렸다. 물론…_medium.com](https://medium.com/@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0-ce659b02b829 "https://medium.com/@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0-ce659b02b829")[](https://medium.com/@moony211/cmake%EB%A1%9C-ffmpeg-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0-ce659b02b829)

앞 선 글에서 제라노 사이트에서 ffmpeg 패키지를 다운로드할 수 있는 방법을 정리했다. 여기에서 불편한 점이 있다. cmake의 build 명령을 실행해야 다운로드가 실행된다는 점, 다운로드 받은 위치가 build하기 위해 지정한 디렉토리의 하부의 깊은 곳에 있다는 점이다.

우선은 cmake의 설정 단계에서 ffmpeg이 다운로드 되도록 시도한다. 버전 정보나 그 밖에 컴파일할 때 환경에 따른 옵션 등을 설정하고자 할 때, configure_file 을 이용한다. 주로 @이나 $ 등을 이용해서 변수 값을 대체하는 형태로 사용하는데, 여기에서는 굳이 이러한 대체 작업은 필요없다. 대신 앞 선 글에서 사용했던 externalProject\_Add를 포함한 cmake 모듈 파일을 생성하도록 만들도록 한다. 거의 동일한 내용이지만, 이 스크립트 자체가 모듈을 생성하는 입력이 된다. 파일 이름은 CMakeLists.txt.in 으로 만든다.

```cmake
cmake_minimum_required(VERSION 3.15)  
project(ffmpeg-download NONE)  
  
include(ExternalProject)  
ExternalProject_Add(ffmpeg  
        URL https://ffmpeg.zeranoe.com/builds/win64/dev/ffmpeg-4.2.2-win64-dev.zip  
        CONFIGURE_COMMAND ""  
        BUILD_COMMAND ""  
        INSTALL_COMMAND ""  
        )
```

환경 설정 과정에서 이 파일이 CMakeLists.txt 로 변환되어 서브 디렉토리에 위치하는 형태로 동작하도록 할 것이다.

```cmake
configure_file(CMakeLists.txt.in ffmpeg-download/CMakeLists.txt)
```

cmake 를 실행시켜보면, ffmpeg-download 라는 디렉토리가 생성되고, CMakeLists.txt 파일이 생성되었음을 확인할 수 있다. 파일은 생성되었지만, 실제 다운로드 과정은 일어나지 않았다. 이것은 어떻게 해야 할까. 이전 글에서도 이 작업은 빌드 과정에서 동작하는 것이었다. 이것을 위한 명령을 추가해 줘야 한다.

```cmake
execute_process(COMMAND ${CMAKE_COMMAND} . WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg-download)  
execute_process(COMMAND ${CMAKE_COMMAND} --build . WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg-download)
```

ffmpeg-download 라는 프로젝트에 환경 설정 과정과 빌드 과정을 함께 실행할 수 있도록 해주는 것이다. 이제는 최상위 프로젝트의 환경 설정 과정에서 ffmpeg을 다운로드 받아서 압축 해제하는 것을 확인할 수 있다.

한 가지 불편한 점이라면 압축 해제한 위치가 너무나 깊은 곳에 위치해 있어서, 라이브러리나 헤더 파일 참조를 할 때 위치 지정하기가 쉽지 않다.

이 점을 개선하기 위해서, 추가적인 수정을 해 보자.

```bash
ExternalProject_Add(ffmpeg  
        URL https://ffmpeg.zeranoe.com/builds/win64/dev/ffmpeg-4.2.2-win64-dev.zip  
        CONFIGURE_COMMAND ""  
        BUILD_COMMAND ""  
        INSTALL_COMMAND "${CMAKE_COMMAND}" -E rename ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg-download/ffmpeg-prefix/src/ffmpeg ${CMAKE_SOURCE_DIR}/ffmpeg  
        )
```

원래의 INSTALL\_COMMAND는 지정하지 않았지만, 이제는 압축 해제된 파일을 다루기 쉬운 위치로 옮기기 위해서 rename 명령을 이용했다. 결과적으로는 압축 해제한 디렉토리를 이동하는 효과를 얻을 수 있다.

[소스 코드](https://github.com/moonyl/link-ffmpeg-with-cmake/archive/v0.2.zip)는 이 곳에 올려두었다. github에서 관리하고 있다.

[**moonyl/link-ffmpeg-with-cmake**  
_cmake를 이용해서 ffmpeg 라이브러리를 자동으로 다운로드하고 링크한다. Contribute to moonyl/link-ffmpeg-with-cmake development by creating an…_github.com](https://github.com/moonyl/link-ffmpeg-with-cmake "https://github.com/moonyl/link-ffmpeg-with-cmake")[](https://github.com/moonyl/link-ffmpeg-with-cmake)