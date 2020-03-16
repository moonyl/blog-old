---
title: "콜백함수 매개 변수에서 destructuring"

categories:
  - web-devel
tags:
  - destructuring
  - callback
  - paremter
  - es6
  - javascript
last_modified_at: 2019-05-10T04:40:00-05:00
---
최근에 react 를 조금씩 보고 있다. 프론트엔드를 대부분 자바스크립트를 이용해서 프로그램할 수 있어서 재미가 솔솔하다. 그러나, 자바스크립트에 대한 경험이 없어서, 처음 보는 문법이 나타날 때마다 엉뚱한 추론 및 삽질이 난무하다. 조금씩 정리해 두던가, 도서를 한번 주욱 훑어서 체계적인 지식 정립이 필요한 시점이 되어 가고 있다.

freeCodeCamp 에 게재된 글을 보다가 다음과 같은 코드를 발견했다. 

```javascript
import Typography from '@material-ui/core/Typography'
import TextField from '@material-ui/core/TextField'
...
handleChange = ({ target: { name, value } }) =>
    this.setState({
      [name]: value
    })
render() {
    const { title } = this.state
    return (
      ...
      <form>
        <TextField
          name='title'
          label='Exercise'
          value={title}
          onChange={this.handleChange}
          margin='normal'
        />
      </form>
    )
  }
}
```

관련 글을 잠깐 링크 걸어두자면 다음과 같다.

[Meet Material-UI - your new favorite user interface library](https://medium.freecodecamp.org/meet-your-material-ui-your-new-favorite-user-interface-library-6349a1c88a8c)

handleChange 로 할당한 것은 화살표 함수로 보인다. 그런데, 매개 변수에 객체 형태처럼 표현한 부분이 있다. 이 부분만 따로 떼어내서 나타내면 다음과 같다.

```javascript
{ target: { name, value } }
```

이것이 무엇이란 말인가. 결론부터 말하면, 자바스트립트에서 destructuring 이라는 방법을 이용해서 표현한 것이다. 

조금 더 파보면, TextField 라는 컴포넌트 내에 onChange 라는 prop 의 콜백함수를 지정할 수 있는데, 여기에 this.handleChange 함수를 설정했다. onChange라는 콜백이 호출될 때는 매개변수로 event 라고 알려진 객체가 넘어올 것이다. 즉, 매개변수에는 event 와 같은 참조할 객체 이름이 와야 한다. 그러나, 뜬금없이 { target: { name, value } } 와 같은 형태로 온 것이다. 

또 다시, 결론적으로 이 표현을 통해서 함수 내에서는 파라메터로 넘겨진 객체의 target 이라는 멤버의 name과 value 값만을 빼내서 사용하겠다는 의도라고 얘기할 수 있다.

이 내용을 확인해보기 위해서 간단한 프로그램을 작성했다.

```javascript
let testObj = { id: 0, target: { name: "name", value: "moony" }};
const testFunction = ({target: { name, value } }) 
  => { console.log("name: ", name, ", value: ", value); }
testFunction(testObj);
```

결과는 짐작한대로 다음과 같이 나온다.

```
name:  name , value:  moony
```

즉, testFunction 의 인자로 testObj라는 객체를 넘겼지만, testFunction 내에서는 testObj의 내부 멤버인 name과 value 만을 추출해서 사용하는 것이다.
destructuring에 대한 설명은 다음의 MDN 페이지에서 확인할 수 있다.

[구조 분해 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)