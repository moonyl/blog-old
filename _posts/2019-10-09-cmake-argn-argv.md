---
title: CMake에서 ARGN과 ARGV

categories:
  - cmake
tags:
  - cmake
  - 빌드
  - ARGN
last_modified_at: 2019-10-09T07:16:00-05:00
---
# CMake에서 ARGN과 ARGV
cmake는 편하기도 하면서, 이해하기 어렵기도 하다. 솔직히 cmake 정식 사이트의 매뉴얼은 읽어도 무슨 말인지 잘 이해되지 않는다. 아직 그만한 내공이 쌓이지 않아서 그런 것 같다.
ARGN과 ARGV도 이해가 잘 되지 않는 항목 중에 하나였다.
> [${ARGV} holds the list of all arguments given to the macro and ${ARGN} holds the list of arguments past the last expected argument.](https://cmake.org/cmake/help/latest/command/macro.html)

ARGV는 확실히 매크로나 함수에 넘겨지는 모든 인수를 의미한다는 것을 알 수 있다. ARGN과 차이는 `past the last expected argument`, 이것에 달려있다. 몇 가지 확인 작업을 거쳐서 차이를 정리한다.
```cmake
function(test a1 a2 a3)
    message(STATUS "ARGV=${ARGV}")
    message(STATUS "ARGN=${ARGN}")
endfunction()

test(a b c)
```
결과가 예측되는가? 확실히 ARGV는 `a1;a2;a3`가 나올 것 같다. 그렇다면 ARGN은? 신기하게도 빈 문자열이 결과로 나온다. ARGN은 그렇다면 무슨 용도가 무엇이란 말인가? 새로운 흥미로운 예를 하나 더 들어보겠다.
```cmake
function(test)
    message(STATUS "ARGV=${ARGV}")
    message(STATUS "ARGN=${ARGN}")
endfunction()

test(a b c)
```
이 경우는 test라는 함수는 파라메터가 없는 것으로 정의를 했지만, 3개의 `기대하지 않은 인자`를 넘겨서 실행한 경우가 되겠다. ARGV는 이전과 동일하게 `a1;a2;a3`를 담게 된다. 그런데 ARGN은 놀랍게도 ARGV와 동일한 값을 담고 있다. 이것이 ARGN이 앞에서 정의한 `the list of arguments past the last expected argument`의 의미라고 생각한다.
그러면, 이것을 어디에 써먹을 수 있겠는가. 개인적인 생각이긴 하지만, 다음과 같은 경우를 생각해볼 수 있다.
```cmake
function(libfun a1 a2)
...어쩌고 저쩌고
endfunction()

function(myfun arg)
...
내 함수에서는 libfun함수의 결과가 필요하지만
a1을 이용해서 함께 결과를 도출한다.
...
    libfun(${ARGN})
endfunction()

myfun(a b c)
```
대충 이러한 형태가 될 것 같다. myfun 함수는 실제로 하나의 인자만 사용한다고 되어 있지만, 내부에서 추가적인 인자를 이용해서 처리하는 경우를 구현하는 형태이다. 그러나, 어떻게 인자를 넘겨야 할지 불분명하기 때문에 사용자에 대한 배려가 없는 코드가 될 확률이 높다.
도대체 왜 ARGN이 있는가? 몇 가지 CMake를 사용하는 코드들을 보면서 추론한 것이지만, 결국 cmake에서 함수로 넘어오는 인자가 가변적이고, 그 수가 매우 많을 수 있다는 점과 이러한 상황을 잘 처리할 수 있는 `cmake_parse_arguments`라는 함수를 자체적으로 제공하고 있어, 이를 이용하는 스크립트가 많기 때문이라고 결론을 내렸다.