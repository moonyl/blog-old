---
title: node.js 에서 import 구문 사용하기
description: >-
  Ecmascript module — 정식 명칭인지는 모르겠다. — 은 node.js에서 기본적으로 지원하지 않는다. 즉, 최근 브라우저에서
  모듈을 가져오거나 내보낼 때 사용하는 import/export 를 node.js에서는 사용할 수 없다는 것을…
date: '2020-02-09T11:19:40.791Z'
categories:
  - nodejs
tags:
  - import
  - babel
  - es6
  - git
last_modified_at: 2020-01-10T00:17:39.290Z
slug: >-
  /@moony211/node-js-%EC%97%90%EC%84%9C-import-%EA%B5%AC%EB%AC%B8-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-77547b079136
---

Ecmascript module — 정식 명칭인지는 모르겠다. — 은 node.js에서 기본적으로 지원하지 않는다. 즉, 최근 브라우저에서 모듈을 가져오거나 내보낼 때 사용하는 import/export 를 node.js에서는 사용할 수 없다는 것을 의미한다.

node.js 에서는 기본적으로 module 을 지원하고 있었으며, common module 방식이라는 이름으로 require/exports 을 이용해 유사한 기능을 할 수 있다.

Ecmascript 표준으로 지원하는 것을 node.js에서 그냥 두고만 보고 있지는 않을 것이고, 앞으로는 기본 지원이 될 것이라 예상한다. 실은 버전 12부터는 experimental-modules라는 옵션을 넘겨주면 사용이 가능하다.

babel을 사용해서 transpile을 통해 import/export 방식의 모듈 사용을 지원할 수도 있다. 여기에서는 이 방법을 기록해두고자 한다. 왜냐하면, 이 방법이 범용적으로 사용될 수도 있고, 특히, 패키징을 할 때 사용할 pkg에서 변환된 코드로 하면, 쉽게 작업을 완료할 수 있기 때문이다.

### babel 설치

우선, babel을 설치해야 한다. babel에 관련된 모듈이 여러가지가 있는데, 가장 핵심은 @babel/core 이다.

명령어 방식으로 transpile하여 특정 위치에 결과 파일을 얻기 위해서는 @babel/cli 가 필요하다.

ES6 이상의 문법으로 작성된 코드를 바로 node 런타임으로 실행하기 위해서는 @babel/node가 필요하다.

babel이 transpile을 하기 위해서는 여러 플러그인이 필요하다. 사용하기 쉽도록 플러그인을 모아둔 것이 있는데, 이것을 preset이라고 부르는 것 같다. @babel/preset-env 을 설치하면 된다.

패키지 매니저를 yarn을 사용하고 있어서, yarn 을 사용하는 방법을 정리한다.

```bash
$ yarn add @babel/core @babel/cli @babel/node @babel/preset-env -D
```

플러그인을 사용하기 위해서 설정을 만들어줘야 한다. 설정 내용은 .babelrc 라는 파일에 기록하면 된다.

```json
{  
  "presets": \["@babel/preset-env"\]  
}
```

### 간단한 테스트 프로그램

import/export를 정상적으로 사용할 수 있는지 확인하기 위해서 간단한 express 앱을 작성했다.

[**moonyl/ecmascript-module-nodejs**  
_nodejs에서 ecmascript module 방식을 사용할 수 있도로 환경을 구성한다. - moonyl/ecmascript-module-nodejs_github.com](https://github.com/moonyl/ecmascript-module-nodejs "https://github.com/moonyl/ecmascript-module-nodejs")[](https://github.com/moonyl/ecmascript-module-nodejs)

프로그램은 별 의미가 없이 “/” 요청을 할 때, bcrypto 모듈을 통해 salt 값을 생성해서 보여주는 기능을 한다. bcrypto 모듈을 사용한 이유는 pkg 모듈로 패키징을 할 때 native addon에 대해서 제약사항이 있는데, 이것을 확인하기 위해서 추가로 기능을 작성했다.

bin 디렉토리에서 runner.js를 통해 실행을 하고, src에 나머지 코드를 작성하는 방식을 채택했다.

실행을 위해서 package.json 파일에 start 스크립트를 추가한다.

```json
"scripts": {  
  "start": "babel-node bin/runner"  
}
```

babel-node 를 사용해서 실행한다.

ecmascript module이 적용된 코드를 node.js에서 실행하는 것으로 만족한다면 여기에서 더 고민할 필요는 없다. 그러나 pkg 를 통해서 패키징을 한 후, 실행을 하고자 한다면, 실제 transpile을 한 결과를 얻고, 패키징을 해야 한다.

```json
"scripts": {  
  "babel:build": "babel bin -d dist/bin && babel src -d dist/src"  
}
```

transpile된 결과를 dist 디렉토리에 얻을 수 있다. pkg 를 이용해서 패키징할 파일은 바로 dist 디렉토리로에 있는 코드로 구하는 것이다.

### pkg로 패키징하기

```json
"scripts": {  
  "build": "pkg . --targets=node12-win-x64"  
}
```

windows 환경에서만 패키징 결과를 얻기 위해서 targets 옵션을 지정했다. pkg 로 패키징 할 때 시드가 될 파일을 설정해야 하기 때문에 “bin:app” 설정도 추가로 필요하다.

```json
"bin": {  
  "app": "dist/bin/runner.js"  
}
```

transpile 되지 않은 파일을 패키징 한다면 “app”에 대한 값으로 “bin/runner.js” 을 설정했을 것이다.

결과 파일로 ${프로젝트 이름}.exe 가 나올 것이고, 이것을 실행하면 node.js 런타임 환경을 구성할 필요없이 바로 실행할 수 있게 된다.

그러나, 이 실행파일을 다른 디렉토리로 옮긴 뒤 실행하면 또 다른 도전 작업이 기다리고 있다.

```
1) If you want to compile the package/file into executable, please pay attention to compilation warnings and specify a literal in 'require' call. 2) If you don't want to compile the package/file into executable and want to 'require' it from filesystem (likely plugin), specify an absolute path in 'require' call using process.cwd() or process.execPath.  
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:795:15)  
    at Function.Module._resolveFilename (pkg/prelude/bootstrap.js:1287:46)  
    at Function.Module._load (internal/modules/cjs/loader.js:688:27)  
    at Module.require (internal/modules/cjs/loader.js:850:19)  
    at Module.require (pkg/prelude/bootstrap.js:1166:31)  
    at require (internal/modules/cjs/helpers.js:74:18)  
    at Object.<anonymous> (C:\\snapshot\\esm-check\\node_modules\\bcrypt\\bcrypt.js:6:16)  
    at Module._compile (pkg/prelude/bootstrap.js:1261:22)  
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:993:10)  
    at Module.load (internal/modules/cjs/loader.js:813:32) {  
  code: 'MODULE_NOT_FOUND',  
  requireStack: [  
    'C:\\snapshot\\esm-check\\node_modules\\bcrypt\\bcrypt.js',  
    'C:\\snapshot\\esm-check\\dist\\src\\first.js',  
    'C:\\snapshot\\esm-check\\dist\\bin\\runner.js'  
  ],  
  pkg: true  
}
```

모듈이 없어서 그렇다는 의미로 보인다. bcrypt는 native addon을 이용하는 모듈인데, 이를 위한 bcrypt\_lib.node 파일이 존재하지 않기 때문에 발생하는 문제로 보인다.

이것을 개선할 수 있는 방법은 node\_modules/lib/binding/bcrypt\_lib.node 파일을 복사해 오는 것이다. 지금까지 알아낸 방법으로는 이것이 최선이다.

즉, pkg로 패키징한 파일과 native addon한 .node 파일을 같은 디렉토리에 위치시키고 실행하면 된다.