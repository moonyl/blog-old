---
title: bsamseth/cpp-project 에서 발견한 흥미로운 git 사용
description: cpp 프로젝트의 기본 틀을 만들어 주는 재미있는 프로젝트가 있다.
date: '2020-01-11T01:20:27.220Z'
categories:
  - tool
tags:
  - cpp
  - cmake
  - doctest
  - git
last_modified_at: 2020-01-10T00:17:39.290Z
slug: >-
  /@moony211/bsamseth-cpp-project-%EC%97%90%EC%84%9C-%EB%B0%9C%EA%B2%AC%ED%95%9C-%ED%9D%A5%EB%AF%B8%EB%A1%9C%EC%9A%B4-git-%EC%82%AC%EC%9A%A9-92f088c21fd0
---

cpp 프로젝트의 기본 틀을 만들어 주는 재미있는 프로젝트가 있다.

[**bsamseth/cpp-project**  
_You can't perform that action at this time. You signed in with another tab or window. You signed out in another tab or…_github.com](https://github.com/bsamseth/cpp-project/blob/master/setup.sh "https://github.com/bsamseth/cpp-project/blob/master/setup.sh")[](https://github.com/bsamseth/cpp-project/blob/master/setup.sh)

cmake, doctest, doxygen 등, 설정하기 귀찮지만 구축해두면 아주 유용한 작업 등 한 번에 완성시켜준다.

클론을 한 뒤에 setup.sh 을 실행하라고 설명이 되어 있는데, 어떤 일을 하는 건지 궁금해서 내부를 들어봤더니, 이런 코드가 있었다.

```bash
git reset "$(git rev-list --max-parents=0 --abbrev-commit HEAD)"
```

대체 이것이 무엇을 하기 위한 명령일까. git의 reset이라는 명령과 rev-list 라는 명령은 아직 써본 경험이 없기 때문에 호기심을 자극했다.

먼저 rev-list 는 commit 해시값을 역순으로 출력해준다. 역순이라고 굳이 표현한 건 rev 라는 단어 때문이다. 결과적으로 최신 commit 버전이 가장 먼저 출력된다.

```bash
git rev-list HEAD

b2005a1ed4bfe85639259f068ab84847229f4456  
ac2f5b6ba5f1f85cf52e5724133a0b845f7f4427  
951b6789d6d207f6c7df70ab6a5dc906dfc13755  
54c12473917c6f50556d753e12d4b22164fff001  
98f49bc5dcdb5e8c38b9db30c1f2485b225dd907
```

abbrev-commit 옵션은 무엇일까? abbrev가 abbreviate의 약자로 예상되는데, commit 길이가 길다보니 줄여서 표시해주는 역할을 한다.

```bash
git rev-list --abbrev-commit HEAD

b2005a1  
ac2f5b6  
951b678  
54c1247  
98f49bc
```

max-parents=0 이라는 옵션을 주면 최상위 커밋만을 보여주는 것 같다. all root commits 라고 매뉴얼에는 설명되어 있는데, 아직 여러 개를 넘겨주는 상황은 보지 못했다.

```bash
git rev-list --max-parents=0 --abbrev-commit HEAD

98f49bc
```

결국, 가장 최초의 커밋 버전을 획득하게 된다.

reset 명령은 현재 브랜치를 지정한 커밋으로 이동시키면서 그 이후의 커밋은 제거하는 역할을 한다. 작업 디렉토리나 stage 의 내용은 그대로 남아 있게 되므로, 최초의 커밋 버전이 있는 상태에서 최신의 작업 내용을 반영할 수 있는 상태로 만들어 주는 역할을 한다.

기본틀을 제공하지만, 최신의 것으로 구축하면서만, 원류가 무엇이다라는 것을 남겨두는 기발한 방법이라 생각된다.

cpp-project 를 바탕으로 프로젝트를 작성해서 git에 반영하면 항상 최초의 로그는 cpp-project의 최초의 것으로 기록되는 것이다. 재미있는 내용이다.