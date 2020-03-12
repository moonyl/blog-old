---
title: react app과 백엔드 서비스 함께 개발하기
date: 2019-10-01 05:47:35
categories:
  - web-devel
tags: 
  - react
  - backend
last_modified_at: 2019-10-01T05:47:35-05:00
---

# react app과 백엔드 서비스 함께 개발하기

create-react-app을 사용하면 react 개발 환경을 쉽게 만들 수 있다. 

```bash
npx create-react-app proxy-verify
```
yarn start를 실행하면 개발 서버가 구동된다. 
```bash
cd proxy-verify
yarn start
```


그런데, 백엔드 쪽에 API도 같이 개발하려고 한다면, create-react-app 에서 제공하는 개발서버로는 용이하지 않다. 백엔드 쪽에도 웹 애플리케이션 서버를 하나 구동해서 API를 제공하고, 이를 react app이 이용하는 경우를 생각할 수 있다. 결국, react app이 특정 API 기능을 요청하게 되면, react 개발 서버는 이 요청을 받아서 API 서버로 다시 요청하는 형태가 되어야 한다. 즉, react 개발 서버는 proxy 로서의 역할을 해야 한다. 다행히도, 이 기능을 제공하고 있다. 이러한 개발 상황이 흔하게 있기 때문일 것이다.

package.json에서 이러한 proxy 설정을 한다.
```json
"proxy": "http://localhost:4000"
```


자세한 내용은 다음 링크를 참조한다.
> [**Proxying API Requests in Development · Create React App**
> *Note: this feature is available with [email protected] and higher. People often serve the front-end React app from the…*create-react-app.dev](https://create-react-app.dev/docs/proxying-api-requests-in-development)

중요한 내용은 정적 자원이 아니면, proxy로 설정된 서버로 요청을 전달한다는 내용이다. 이것을 확인해보기 위해서 server라는 디렉토리를 만들고, express를 설치한다.
```bash
mkdir server
cd server
```


아주 심플한 API 서버를 하나 구현한다. server.js에 코드를 작성한다.
```javascript
const express = require("express");
const app = express();

app.get("/api", function(req, res) {
    const reply = { message: "api test message" };
    res.send(reply);
});


app.listen(4000, function() {
    console.log("Example app listening on port 4000!");
});
```

react app 에도 API 를 호출해서 간단히 결과를 보여줄 수 있는 기능을 하나 구현한다.
```jsx
import React from "react";
import "./App.css";

class Fetch extends React.Component {
    state = { result: null, error: null };

    componentDidMount() {
        fetch("/api")
        .then(res => {
            console.log({ res });
            return res.json();
        })
        .then(result => {
            console.log(result);
            this.setState({ result });
        })
        .catch(error => {
            console.error(error);
            this.setState({ error });
        });
    }

    render() {
        return this.props.children(this.state);
    }
}

function App() {
    return (
        <Fetch>
            {({ result, error }) => {
                console.log({ result, error });
                return (
                    <div>
                        {result && <p>{result.message}</p>}
                        {error && <p>{error}</p>}
                    </div>
                );
            }}
        </Fetch>
    );
}

export default App;
```


API 서버를 구동하는 스크립트를 package.json에 추가한다. react app을 구동하는 스크립트의 이름도 같이 수정해둔다.
```json
...
"scripts": {
    "start:client": "react-scripts start",
    "start:server": "nodemon server/server.js",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
},
...
```


node 로 그냥 시작해도 되지만, 이왕이면 코드가 수정될 때 새롭게 실행되도록 nodemon을 사용한다.

nodemon을 설치하지 않았다면 다음으로 추가할 수 있다.
```bash
yarn global add nodemon
```

이제 react app과 API 서버를 각각 실행한다. 실행하면, 그 프로세스가 터미널을 독점하기 때문에, 터미널 프로그램을 두개 띄워놓고 각각 실행해야 한다.

첫번째 터미널에서,
```bash
yarn start::server
```

두번째 터미널에서,
```bash
yarn start::client
```

자동으로 웹브라우저가 뜨면서 localhost:3000으로 접속된다. 직접 웹브라우저에서 localhost:3000을 입력해도 된다. react app에서 API 서버로 요청을 한 것이 아니라 /api 를 요청했지만, proxy 설정으로 인해, localhost:4000으로 연결되어 처리된다.

이 정도로 개발환경을 꾸며뒀더라도, 충분히 편해지긴 했지만, 처음에 실행할 때 터미널 두개 띄우고, 각각을 실행해야 한다는 것도 번거롭다. 병렬적으로 여러개를 실행할 방법은 없을까. npm-run-all 을 사용하면 된다. 우선 npm-run-all을 설치한다.
```bash
yarn add npm-run-all --dev
```

package.json에 start 스크립트를 추가한다. 앞에서 start:server, start:client 스크립트로 나눈 의도가 바로 여기에 있다.
```json
...
"scripts": {
    "start": "npm-run-all -p start:*"
    "start:client": "react-scripts start",
    "start:server": "nodemon server/server.js",
...
```


‘-p’ 라는 옵션은 병렬적으로 실행한다는 의미이다. 순차적으로 실행하면, 각 스크립트가 터미널을 독점하기 때문에 하나가 실행되고 멈춰버린다.

이제는 한 번에 두개의 스크립트를 실행할 수 있다.
```bash
yarn start
```
