---
title: "C ++ 모듈로 Native Node.js 만들기 튜토리얼\_. 1 부 — Nan 소개"
description: >-
  이 글은 “Tutorial to Native Node.js Modules with C++. Part 1 — An Introduction to
  Nan” 을 번역한 글입니다.
date: '2018-11-04T01:04:24.736Z'
categories:
  - nodejs
tags:
  - C
  - native binding
  - Nan
  - nodejs
last_modified_at: 2018-11-04T01:04:24.736Z
slug: >-
  /@moony211/c-%EB%AA%A8%EB%93%88%EB%A1%9C-native-node-js-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-1-%EB%B6%80-nan-%EC%86%8C%EA%B0%9C-e5350cb2b5f7
---

이 글은 “[Tutorial to Native Node.js Modules with C++. Part 1 — An Introduction to Nan](https://medium.com/netscape/tutorial-building-native-c-modules-for-node-js-using-nan-part-1-755b07389c7c)” 을 번역한 글입니다.

연산 비용이 많이 들거나 메모리 사용량이 많은 작업을 하기 위한 모듈을 만들고 싶다고 가정해봅시다. 분명, 자바스크립트는 이러한 용도에는 적합한 언어는 아닙니다. 또 다른 상황을 가정해봅시다. 이번에는 자바스크립트와 함께 좋아하는 C++ 라이브러리를 사용하려고 합니다. 두 경우 모두에 대해서, Node.js를 이용해서 자바스크립트로 어떠한 C++코드나 라이브러리와 연결(바인딩)할 수 있습니다.

그러한 작업을 하는 이유는 여러 가지가 있습니다. 다음은 내가 OpenCV C++ 라이브러리와 자바스크립트 바인딩으로 **npm 패키지**를 작성하게 된 이유입니다.

*   웹 응용 프로그램에서 OpenCV 사용하기 위해서
*   OpenCV로 electron 응용 프로그램을 만들기 위해서(QT에서 골머리를 앓느니 HTML과 CSS 또는 React로 멋진 GUI를 만드는 것을 선호함)
*   Mocha와 Chai로 단위 테스트를 하기 위해서
*   자바 스크립트 코딩 작업은 멋지다 — 가장 분명한 이유입니다.

그래서, 네이티브 바인딩을 개발하는 것으로부터 시작하고 싶지만, 어디에서 시작해야할 지 모르겠다라면 걱정 없어요. 몇 분 만에 방법을 가르쳐 드릴겁니다.

#### 또 다른 v8과 Nan 튜토리얼이 있나요?

네이티브 node 애드온을 개발하는 방법에 대한 튜토리얼을 읽은 후에도 여전히 많은 의문이 있었습니다. 이 글을 쓰는 와중에도 v8과 Nan에 처음 마주쳤을 때 머리를 감싸고 이해하는 데 어려움을 겪었던 문제들에 주력을 해야 했습니다. 다행히도 이 방법으로 쉽게 시작할 수 있습니다. 언제나 그렇듯, 이 예제의 소스 코드는 내 [**github 저장소**](https://github.com/justadudewhohacks/node-addon-tutorial)에 있습니다.

### 프로젝트 설정

이 예제에서는 헤더 파일의 집합인 Nan (Native Abstractions for Node.js)을 사용합니다. Nan는 헤더 파일의 집합이며, node 애드온을 더 쉽게 개발할 수 있게 하고, 다른 node 버전 간에 호환성을 유지할 수 있게 해주는 헬퍼와 매크로들을 제공합니다. package.json은 다음과 같습니다:

<script src="https://gist.github.com/justadudewhohacks/2a358c7dc94eb2eae79039bd9235be6c.js" charset="utf-8"></script>

Nan을 dependency로 지정하고 node-gyp build와 node-gyp rebuild 스크립트 (깨끗하게 다시 빌드를 위한)를 추가합니다. Node-gyp은 컴파일 된 C ++ 코드를 JS 애플리케이션에 필요할 수있는 .node 파일로 묶을 수있게 해주는 모듈입니다. 최소한 우분투에서 최신 버전의 node.js가 있어야 합니다. 그렇지 않다면 간단히 `npm i -g node-gyp`만 입력하면됩니다. Windows 사용자는 시스템에 msvc14 (Visual Studio 2015) 빌드 도구를 설치해야합니다. 다행스럽게도 같은 목적으로 npm 패키지가 있습니다. `npm i -g windows-build-tools` 을 실행합니다.

또한 패키지의 엔트리 파일이 bindings.js 이라는 것에 주의할 필요가 있습니다. 이 파일에서 .node 모듈로의 경로를 지정하게 됩니다.

<script src="https://gist.github.com/justadudewhohacks/bb30e2023539bc6900ec45162eddd909.js" charset="utf-8"></script>

node-gyp으로 모듈을 빌드하기 위해서는 binding.gyp 파일로 관련 설정을 하면 됩니다.

모든 헤더와 소스 파일을 src 폴더에 넣고 node\_modules에 설치된 Nan 헤더를 include 경로에 추가합니다. 소스 아래에서 모듈로 컴파일 할 각 .cc 파일을 지정해야합니다. 여기에서 해 볼 간단한 예제에서는 Vector 클래스를 만들고 index.cc 에서 이 클래스를 표출할 것입니다. 일반적으로 index.cc에서 모든 클래스와 cc 모듈을 포함하고 초기화합니다. 이 모듈은 “Init” 메소드를 구현해서, 인터페이스를 모듈에 노출시킵니다.

<script src="https://gist.github.com/justadudewhohacks/cd72179e7c9e6c6ac4d00365977f6ba2.js" charset="utf-8"></script>

C ++ 모듈을 빌드하고 공개하기 위한 설정 작업이 끝났습니다. 이제는 구현 작업을 시작하기만 하면 됩니다!

### 첫 번째 클래스 구현

아마도 처음 시작할 때 가장 중요한 것은 네이티브 측 상에 클래스를 작성하는 방법과 자바스크립트 어플리케이션에서 그것을 사용하는 방법을 이해하는 것입니다. 다음을 실행할 수 있는 간단한 Vector 클래스를 구현한다고 가정하겠습니다.

<script src="https://gist.github.com/justadudewhohacks/f29cd2715c498a125621c55ac5211913.js" charset="utf-8"></script>

이제 x, y, z 좌표로 새로운 벡터를 초기화하려고 합니다. 새로운 값을 할당하여 좌표를 변경하고 마지막에는 다른 벡터와의 합을 구하려고 합니다.

#### 클래스 선언

Vector 클래스를 정의하기 위해 다음과 같은 내용으로 헤더 파일 “Vector.h”를 만듭니다.

<script src="https://gist.github.com/justadudewhohacks/a787f65529621ff504f71b43b8733d9e.js" charset="utf-8"></script>

Vector가 Nan :: ObjectWrap 클래스를 상속하고 있다는 것을 알 수 있습니다. 자바 스크립트 앱과 네이티브 모듈을 래핑하여 JS 측으로 넘기고 그것을 풀어서 네이티브 측에서 검색하기 때문에, 이 클래스의 인스턴스를 앱과 모듈 간에 전달하는 데에 필요합니다. 이 튜토리얼 뒤에서 객체 래핑에 대해 알아볼 것입니다.

또한 클래스에는 x, y, z의 세 가지 속성과 값에 대한 getter 및 setter가 있습니다. 이것은 **_NAN\_GETTER_** 및 **_NAN\_SETTER_** 매크로를 사용하여 선언합니다. **_Init_**, **_New_** 및 **_Add_** 메소드도 선언합니다. 이름에서 알 수 있듯이 **_New_** 함수는 생성자가 되고, **_Init_** 함수는 앱에서 모듈을 요청할 때 가장 먼저 호출되는 것인, “module.exports” 가 됩니다. 기본적으로 Vector 클래스를 모듈에 노출시키고 **_NAN\_MODULE\_INIT_** 매크로로 선언합니다. 자바스크립트에서 접근할 수있는 메소드는 **_NAN\_METHOD_**로 선언됩니다. 자바스크립트에서 호출할 수 있는 것은 모두 정적으로 선언해야합니다.

클래스 헤더에서 마지막으로 정의해야 할 것은 생성자에 대한 핸들을 유지하는 것입니다. 핸들의 수명을 결정하는 로컬 핸들과 영속적 핸들을 일반적으로 다루게 될 것입니다. 영속적 핸들과 대조적으로, 로컬 핸들 (나중에 많이 보게 될 것입니다)은 선언된 범위를 벗어난 후에는 가비지 콜렉트되어 처리됩니다. 하지만 핸들과 범위에 대해서는 걱정하지 마십시오. **_NAN\_METHOD_**가 친절하게도 이미 이 처리를 해주기 때문에 자바스크립트에서 접근 가능한 메소드 내에서 **_HandleScopes_**를 선언하는 것조차 필요가 없습니다.

#### 클래스 메소드 구현하기

클래스를 선언 했으므로 이제 Vector.cc 파일에 추가할 메서드를 구현할 차례입니다.

**Init:**

<script src="https://gist.github.com/justadudewhohacks/c665eb6eaf9be27e8bdcc5eafe900321.js" charset="utf-8"></script>

이것은 새로운 클래스를 초기화하기 위해 튜토리얼에서 채택한 기본 패턴입니다. 먼저 생성자에 대한 FunctionTemplate을 만듭니다. 이 함수를 선언한 **_New_** 메서드에 할당하고 **_Reset_**을 호출하여 이 핸들을 영구적인 생성자에 할당합니다.

또한 클래스 이름, 접근자 및 프로토타입 메소드를 설정합니다. **_SetClassName_**과 **_Nan::SetAccessor_**는 속성 이름이 자바스크립트 문자열인 **_v8::Local<v8::String>_**이 ​​될 것으로 기대합니다. **_Nan::New_**가 **_v8::Local_** 핸들러를 싸고 있는 래퍼인 **_MaybeLocal_**을 반환하기 때문에 우리가 **_Nan::New_**로 생성한 모든 JS 객체 상에서 **_ToLocalChecked()_**를 호출해야하는 이유가 있습니다. JS 객체는 비어있을 수 있습니다. **_MaybeLocals_**는 **_IsEmpty()_**를 호출하여 확인해서 예외를 먼저 잡을 수 있습니다. 이 경우에는 핸들을 직접 만들어 **_ToLocalChecked()_**를 안전하게 호출하여 **_v8::Local_** 핸들을 반환 할 수 있습니다. 마지막으로 클래스를 모듈에 표출합니다 (대상은 **_NAN\_MODULE\_INIT_** 매크로에서 가져옵니다).

**생성자:**

<script src="https://gist.github.com/justadudewhohacks/3fe5a927b5eccc08c10046e99c682757.js" charset="utf-8"></script>

여기서 가장 먼저 얘기할 내용은 **_NAN\_METHOD_** 매크로의 **_info_** 객체는 기본적으로 그 함수에 전달된 인수에 대한 정보를 담고있는 객체입니다. 주목해야 할 정보 객체의 일부 속성은 다음과 같습니다.

*   함수 호출에 대한 인자 수: **_info.Length()_**
*   인자 접근: **_info\[0\], info\[1\], … info\[n\]_**
*   함수 호출에 대한 반환값 설정: **_info.GetReturnValue().set(…)_**
*   **_new_**로 호출된 생성자인가?: **_info.IsConstructCall()_**
*   프로토타입 메소드 호출의 인스턴스: **_info.This()_**
*   생성자에서 사용해야하는 “this”: **_info.Holder()_**

생성자에서 우리는 3개의 인자를 갖는 새로운 인스턴스를 생성하는 것과 같은 조건을 검사합니다. 각각의 인스턴스는 숫자가 아닌 문자열이고, 정의되지 않은 것입니다. 주의할 것은 **_Nan :: ThrowError_**는 JS 측에서 catch 될 수 있는 오류를 발생시키지만, C++ 함수는 종료되지 않도록 명시적으로 반환합니다.

더 흥미로운 일은 18 번째 줄에서 즉, 새로운 인스턴스를 만드는 곳에서 일어납니다. 우리는 새로운 Vector를 생성하고 **_info.Holder()_**로부터 JS 인스턴스로 Vector 상에서 **_Wrap_**을 호출합니다. Vector 클래스는 **_Wrap_**  메서드를 제공하는 **_Nan::ObjectWrap_**을 상속한다는 것을 떠올려 봅시다. 그런 다음 벡터의 필드 값을 초기화하고 JS 쪽으로 반환합니다.

Add:

<script src="https://gist.github.com/justadudewhohacks/092969fb7e49560897b808aefcdd2a20.js" charset="utf-8"></script>

Add 메서드의 구현에서는 **_Nan :: ObjectWrap::Unwrap_**을 호출하여 풀어내는 작업이 어떻게 수행되는지 확인할 수 있습니다. ‘vecSum = vec1.add(vec2)’와 같은 메소드를 호출하고자 했습니다. 3행에서 인스턴스를 풀어내어 (vec1) 상에 메소드를 호출합니다. 이 인스턴스는 **_info.This()_**에서 가져옵니다. Vec2는 인수 0으로 전달되므로 9 행의 **_info\[0\]_**에서 풀어냅니다. 생성자를 사용하여 **_v8::Value_** 가 실제로 클래스의 인스턴스인지를 확인할 수 있습니다.

네이티브 측에서 새로운 인스턴스를 생성하기 위해서, 인자 갯수인 argc와 인자의 배열로 생성자를 호출해야 합니다. 그러므로 생성자 함수인 Nan::NewInstance를 호출 할 것이고 argc와 두 Vector의 x, y, z 의 합을 v8::Values의 배열(22 행)으로 해서 호출할 것입니다. 그러면 Vector의 JS 인스턴스를 래핑할 수 있습니다. 그런 다음 새 JS 인스턴스를 반환할 수 있습니다.

**접근자:**

마지막으로 접근자를 구현합니다. **_NAN\_GETTER_** 및 **_NAN\_SETTER_**에서는 **_v8::String_**으로 전달 된 **_property_** 객체로부터 대상 속성 이름을 검색 할 수 있습니다. 이 속성 개체는 **_std::string_**으로 변환됩니다. getter 핸들러 안에서 단순히 어떤 속성이 대상으로 되어 있는지 확인하고 값을 반환합니다.

<script src="https://gist.github.com/justadudewhohacks/c4d7a53402f474e942fb02025475c64a.js" charset="utf-8"></script>

setter 핸들러도 거의 비슷합니다. 유일한 차이점은 값이 v8::Value로 전달된다는 것입니다. 이것은 인스턴스의 값을 설정하기 전에 타입을 확인할 수 있습니다.

취향에 따라 각 속성에 대한 getter 및 setter를 별도로 구현할 수도 있습니다. 이 경우에는 속성이 암시적으로 결정되므로 속성 이름을 확인할 필요가 없습니다.

### 빌드하고 실행하세요!

이미 구현을 완료했습니다. 이제는 실행할 시간.

```bash
npm install &&npm start
```

작은 js 예제를 실행하면 예상되는 결과가 나오게 됩니다.

```bash
\> node ./index.js

vec1 Vector { z: 0, y: 10, x: 20 }  
vec2 Vector { z: 100, y: 0, x: 30 }  
vecSum Vector { z: 100, y: 10, x: 50 }
```

배열, JSON 객체 및 콜백을 처리하는 방법을 알고 싶습니까? [파트 2](https://medium.com/@muehler.v/tutorial-to-node-js-native-c-modules-part-2-arrays-json-and-callbacks-9b81f09874cd)를 계속하십시오.

비동기 node 모듈을 구축하고 싶습니까? [파트 3](https://medium.com/@muehler.v/tutorial-to-native-node-js-df4118efb678)을 확인하십시오.