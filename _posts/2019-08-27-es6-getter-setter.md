---
title: ES6 getter, setter
date: '2019-08-27 09:08:00'
categories:
  - web-devel
tags: 
  - react
  - backend
last_modified_at: 2019-08-27T09:08:00-05:00
---

```javascript
class User {
  constructor(name, age, email) {
    this._name = name; 
    this._age = age; 
    this._email = email; 
  } 
} 

const jeff = new User("Jeff", 30, "jeff@gmail.com");
```

User 라는 클래스를 하나 정의했다. 이 클래스를 객체로 하나 생성한 것이 jeff이다.

자바스크립트에서 아직 접근 권한자와 같은 - C++에서는 public, protected, private 과 같은 형태로 사용한다. - 사양을 제공하지 않는다. 그래서,

```javascript
jeff._name = "Jeff1";
```

과 같은 필드 접근이 가능하다.

그렇지만, 객체 지향 프로그래밍에서의 캡슐화를 적절히 이용하는 것은 중요하다. 아마도 추후에는 접근 권한 사양이 들어갈 것이라고 기대한다. 흥미롭게도 getter, setter 기능은 제공한다.

```javascript
class User {
  ... 
  
  get name() {
    return this._name; 
  } 
  
  set name(name) {
    this._name = name; 
  } 
}
```

이제, _name 이 아닌, name으로 접근이 가능하게 되었다.

```javascript
console.log(jeff.name); 
jeff.name = "Jeff1"; 
console.log(jeff.name);
```