---
title: "cmake 실행 방법을 통해 알게 된 것"

categories:
  - cmake
tags:
  - cmake
  - build
  - install
last_modified_at: 2019-04-18T04:40:00-05:00
---
[Running CMake](https://cliutils.gitlab.io/modern-cmake/chapters/intro/running.html)

cmake를 통해서 나오는 결과물은 Unix 계열의 경우 Makefile이고, 윈도우의 경우에는 .sln 파일이다. cmake 명령을 실행하게 되면, 빌드에 필요한 환경 변수들을 캐시파일에 갱신하는 작업을 하게 된다. 결과물을 가지고, 각 빌드 시스템에 맞는 명령을 실행해줘서 컴파일 및 링크 작업을 할 수 있게 된다. 이 명령이 make 혹은 msbuild가 될 것이다.
그런데, cmake에서 바로 이 빌드 작업을 할 수 있다. `--build` 옵션이 바로 이것이다.

```bash
cmake --build .
```
를 실행하는 것은,

```bash
make
```

를 실행하는 것과 동일하다.
빌드 시스템에는 타겟이라는 개념이 있다. 이 타겟을 통해서 특정 결과물만 얻거나, 빌드를 하면서 나온 중간 과정물을 제거하거나, 설치 등의 작업을 할 수 있게 된다. 이것또한 cmake 명령에서 바로 할 수 있다.

```bash
cmake --build . --target install
```

를 실행하는 것은,

```bash
make install
```

과 동일하다.