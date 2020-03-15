---
title: av_frame_free에 대한 고찰
description: av_frame_free() 가 처리하는 범위
date: '2017-12-20T00:06:59.567Z'
categories:
  - C++
tags:
  - ffmpeg
  - avformat
  - free
  - av_frame_free
last_modified_at: 2017-12-20T00:06:59.567Z
slug: >-
  /@moony211/av-frame-free%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0-8c0b416e2082
---

### av\_frame\_free() 가 처리하는 범위

```cpp
AVFrame *frame = av_frame_alloc();
int ret = av_image_alloc(frame->data, frame->linesize, 1920, 1080, 
  AV_PIX_FMT_RGBA, 32);
```

av\_frame\_alloc() 에서는 AVFrame 크기의 메모리를 할당하고 기본값을 설정한다. 실제 sizeof(AVFrame) 을 통해서 크기를 알아보면 384 바이트이다.

av\_image\_alloc() 에서는 frame->data 가 가리키는 메모리 배열에 format 에 따라 영상 데이터 크기만큼 메모리를 할당한다. AV\_PIX\_FMT\_RGBA 의 경우에는 data 의 메모리 배열에서 하나만 사용한다. 한 라인의 데이터 크기값은 linesize 배열에 data 메모리 배열에 매핑하여 저장된다. 현재 설정대로라면, 한 라인에 1920 pixel 에, RGBA 형태로 한 pixel 당 4 바이트를 차지하므로, 1920 x 4 = 7680 이 linesize\[0\] 에 저장될 것이다.

```cpp
av_frame_free(&frame);
```

이 함수는 av\_frame\_alloc() 으로 할당된 부분만 해제 및 정리를 해준다. frame->data 를 같이 해제해주면 좋겠지만, 그렇지 않다. 해주지 않을 경우에는 당연히 memory leak이 발생한다.

```cpp
av_freep(&frame->data[0]);
```

av\_frame\_free() 를 호출하기 전에 data 메모리 배열부터 해제해줘야 한다. av\_frame\_free() 는 data 메모리 배열을 해제하는 작업을 하지 않는다.