---
title: avformat_seek_file 좀 더 파기
description: >-
  두번째 인자는 time base 참조로 사용할 stream index 이다. -1을 주게 되면 AV_TIME_BASE 단위로 검색을 하게
  된다. 즉, micro second 을 기준으로 하는 값이다.
date: '2017-12-20T00:03:03.665Z'
categories:
  - C++
tags:
  - ffmpeg
  - avformat
  - seek
  - avformat_seek_file
last_modified_at: 2017-12-20T00:03:03.665Z
slug: >-
  /@moony211/avformat-seek-file-%EC%A2%80-%EB%8D%94-%ED%8C%8C%EA%B8%B0-6b5006375c3e
---

```c
avformat_seek_file(m_pAVFormatCtx, -1, 
  seek_min, seek_target, seek_max, m_seekFlags);
```

두번째 인자는 time base 참조로 사용할 stream index 이다. -1을 주게 되면 AV\_TIME\_BASE 단위로 검색을 하게 된다. 즉, micro second 을 기준으로 하는 값이다.

세번째와 다섯번째 인자는 검색할 영역을 지정한다. 두번째 인자로 지정한 stream index 의 time base 단위로 값이 지정되어야 한다.

네번째 인자가 목표로 하는 시간값이다. 역시 두번째 인자로 지정된 time base 단위의 값이다.

다섯번째는 flag 인데, 영상을 기준으로 찾는다면, AVSEEK\_FLAG\_FRAME 가 가장 효율적인 방법일 것 같다. AVSEEK\_FLAG\_BACKWARD 는 무시가 되고, 그 외는 차이를 잘 모르겠다.

두번째 인자로 -1 이 아닌 특정 stream index를 지정하게 되면, 세번째부터 다섯번째 인자는 그 스트림에 기반한 timestamp 값으로 지정해줘야 한다. 이 값은 av\_rescale 함수와 stream 의 time\_base 값을 이용해서 구할 수 있다.

```c
int64_t seek_target = av_rescale(m_seekPos / 1000),	
  m_pAVFormatCtx->streams[0]->time_base.den,	
  m_pAVFormatCtx->streams[0]->time_base.num) / 1000;
```

m\_seekPos 값이 AV\_TIME\_BASE 단위의 값이라고 했을 때, 사용한 함수 사용법이다. AV\_TIME\_BASE 를 단위로 한 값이 micro second 값이므로 그 값이 매우 큰 값이 될 수 있다. 그래서, 1000으로 나눈 값을 이용해서 변환했다. 오차를 줄이기 위한 tip 이라고 볼 수 있다. 계산한 뒤 다시 1000으로 나누는 것은 seek\_target 값은 초단위 값에서 stream 의 time base로 변환한 값을 이용해야 하기 때문이다.