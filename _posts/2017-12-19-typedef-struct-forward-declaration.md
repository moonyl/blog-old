---
title: Typedef된 struct 에 대한 forward declaration
description: typedef 된 struct 에 대한 forward declaration 은 다음처럼 해주면 된다.
date: '2017-12-19T23:56:18.811Z'
categories:
  - C++
tags:
  - typedef
  - forward
  - declaration
last_modified_at: 2017-12-14T21:37:23.794Z
slug: >-
  /@moony211/typedef%EB%90%9C-struct-%EC%97%90-%EB%8C%80%ED%95%9C-forward-declaration-292882a3d728
---

typedef 된 struct 에 대한 forward declaration 은 다음처럼 해주면 된다.

```
typedef struct {...} A;
```

에 대해서, 우선 struct 에 이름을 부여해줘야 한다.

```
typedef struct _A {...} A;
```

그리고, header 파일에서 다음과 같이 forward declaration 을 해준다.

```
struct _A;typedef struct _A A;
```