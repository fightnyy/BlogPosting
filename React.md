# 리액트를 사용하는 이유

1. 가독성
2. 재사용성
3. 유지보수



즉, 정리정돈의 도구라고 생각해도 좋다.

## react 사용하기

* `npm install -g create-react-app` 로 설치한다.
  * `-g` 는 global 하게 설치하겠다는 의미이다.(컴퓨터 어디서나 실행할 수 있도록 하는것)
* `react` 를 실행할 디렉토리를 하나 만든다. (디렉토리 명과 경로는 /Desktop/react 로 하겠다.)
* 해당 경로로 이동한다. 
  * `cd /Desktop/react`
* `react`를 설치한다.
  * `create-react-app .` (여기서 . 는 현재 위치에서 실행하겠다는 의미이다.)



## 배포

* 우리가 원래 리액트 앱을 킬 때는 `npm run start` 를 사용하였다. 
* 하지만 `Empty Cache and Hard Reload` 를 하고 난뒤에 Network에서 리소스를 보면 약 2MB 정도의 크기인것을 확인할 수 있다.
  * 아무것도 없는데도 불구하고......
  * 이렇게 배포하면 안된다. 보안적인 이슈도 있고 앱이 커지면 너무 무겁다.
* 이를 해결하기 위해 실제로 배포할 때는 build 에 있는 내용을 사용해야한다.
  * `build` 는 `npm run build` 를 사용하면 된다. 
  * 실제로 `index.html` 를 확인해보면  엄청 최적화가 되어서 읽을 수도 없다!
* 또한  `build` 를 하고 나면 `serve -s build` 를 하라는 명령어가 출력된다.
  * 이를 설치하기 위해선 `npm install -g serve`  또는 `yarn global add serve` 를 사용하면 된다.
  * `-s build` 의 의미는 `build` 라는 디렉토리를 document root 로 하겠다는 의미이다.
  * 실제로 resource를 보면 147KB 밖에 되지 않는다.



## Component 만들기

```react
class Subject extends Component { // Component는 무조건 대문자로 시작해야함
    render() { // 이는 함수지만 최신 JS에서는 class 안에 있는 함수는 function을 생략할 수 있다.
        return (
            <header>
                <h1>WEB</h1>
                world wide web!
            </header>
        );
    }
}
```

* return에 들어갈때 최상위 태그는 하나여야 한다. 
  * 여기서는 `header` 가 최상위 태그
* 웹 브라우저는 리액트라는것을 모른다.
* 리액트는 유사 자바스크립이다.
  * `<>` 이런거 없음
  * 우리가 `jsx(<div> => "<"div">" )` 로 코드를 작성하면 `create react app` 이 알아서 자바스크립트 코드로 컨버팅 해주는 거임

* 그런데 해당 방법으로는 유동적이게 `Component` 를 구성할 수 없다.

  * 즉, 내가 `h1` 태그 안에 있는 내용을 바꿀 수 없다는 의미이다.

* 이때 `props` 를 사용하면 이를 해결 할 수 있다. 아래의 예시를 보자

  ```react
  class Subject extends Component { // Component는 무조건 대문자로 시작해야함
      render() { // 이는 함수지만 최신 JS에서는 class 안에 있는 함수는 function을 생략할 수 있다.
          return (
              <header>
                  <h1>{this.props.title}</h1>
                  {this.props.sub}
              </header>
          );
      }
  }
  
  class App extends Component{
    render() {
      return (
          <div className="App">
              <Subject title="WEB" sub="world wide web"></Subject>
              <TOC></TOC>
              <Content></Content>
          </div>
      );
    }
  }
  ```

  * 즉, `react` 에서 속성(`attribute`)를 `props` 라고 하는것을 알 수 있다. 



## 외부에서 코드 가져다 쓰기

* ```react
  import React, {Component} from "react";
  
  class TOC extends Component{
      render() {
          return (
              <nav>
                  <ul>
                      <li><a href="1.html">HTML</a></li>
                      <li><a href="2.html">CSS</a></li>
                      <li><a href="3.html">JavaScript</a></li>
                  </ul>
              </nav>
          );
      }
  }
  
  export default TOC;
  ```

  * 이렇게 사용하면 외부에서 `TOC` 를 사용할 수 있다. 

## 참고

### npm 과 npx 의 차이

* `npx` 는 패키지 관리 모듈이 아니라 `npm` 을 더 편하게 사용하기 위해 `npm` 에서 제공해주는 하나의 도구
  * `npm`  > 5.2.0 이면 `npx` 를 사용할 수 있다.
* `npx` 는 `create-react-app` 과 같은 패키지를 임시로 설치해서 딱 한번만 실행시키고 지우는 역할 
  * 장점 
    *  컴퓨터의 공간을 낭비하지 않는다.
    * 실행할 때 마다 최신상태임을 보장 한다. 

## React의 `Component` 의 `state` 또는 `props` 가 변경되면 `render()` 함수가 다시 호출된다.  

* 즉, `props` 나 `state` 가 바뀌면 다시 그려진다.
* 따라서 그 하위에 있는 태그가 있다면 이것의 `render` 도 다시 호출 된다.