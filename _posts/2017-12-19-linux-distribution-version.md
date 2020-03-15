---
title: 리눅스 배포판 버전 확인
description: >-
  리눅스를 직접 설치하지 않고 사용하는 경우, 특히 요즘은 클라우드를 통해서 리눅스 환경을 제공받기 때문에, 어떤 배포판인지 확인하는 것이
  필요할 때가 있다.
date: '2017-12-19T23:59:22.307Z'
categories:
  - linux
tags:
  - linux
  - distribution
  - version
last_modified_at: 2017-12-14T21:37:23.794Z
slug: >-
  /@moony211/%EB%A6%AC%EB%88%85%EC%8A%A4-%EB%B0%B0%ED%8F%AC%ED%8C%90-%EB%B2%84%EC%A0%84-%ED%99%95%EC%9D%B8-ad5e353e8daf
---

리눅스를 직접 설치하지 않고 사용하는 경우, 특히 요즘은 클라우드를 통해서 리눅스 환경을 제공받기 때문에, 어떤 배포판인지 확인하는 것이 필요할 때가 있다.

```
cat /etc/*release
```

/etc 디렉토리에 os-release 와 lsb-release 와 같은 파일이 있다. 이 내용 안에 배포판과 버전 정보를 확인할 수 있다.

lsb 는 Linux Standard Base 의 약자이다.