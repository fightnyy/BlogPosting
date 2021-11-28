# React 2장

## State

* `State` vs `Props`

  * `Props` 는 사용자가 `Component` 를 사용하는 입장에서 중요한 것
  * `State` 는 내부의 구현에 필요한 데이터들
    * 사용자한테는 알 필요도 없고 알아서도 안되는 것들. 컴포넌트 내부적으로 사용되는 것들

  ```java
  class App extends Component{
    constructor(props) {
        super(props);
        this.state={
            subject:{title:'WEB', sub:'World Wide Web'}
        }
    }
    render() {
      return (
          <div className="App">
              <Subject title="WEB" sub="world wide web man!"></Subject>
              <TOC></TOC>
              <Content title="HTML" desc="HTML is HyperText Markup Language."></Content>
          </div>
      );
    }
  }
  
  
  // 내가 하려고 하는것은 state 값을 초기화 하려고 하는 것이다.
  // 그러기 위해선 render라는 함수보다 먼저 초기화 되어야 하는데 그러기 위해선 constructor를 사용해야 한다. 
  ```

  * 나는 여기서 `state` 를 `props` 의 값으로 주고 싶은 것이다. 이럴땐 아래와 같이 코드를 구성하면 된다.

  ```java
  class App extends Component{
    constructor(props) {
        super(props);
        this.state={
            subject:{title:'WEB', sub:'World Wide Web'}
        }
    }
    render() {
      return (
          <div className="App">
              <Subject title=this.state.subject.title sub=this.state.sub></Subject>
              <TOC></TOC>
              <Content title="HTML" desc="HTML is HyperText Markup Language."></Content>
          </div>
      );
    }
  }
  
  //react의 props 에서 ""로 묶어주면 이는 문자열로 된다. 단, {} 로 해주면 이는 문자열이 아니다.
  ```

* 여기서 우리는 `state`의 역할을 알 수 있다.

  ```react
  ReactDOM.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
    document.getElementById('root')
  );
  
  // 여기서 보면 지금 <App> 태그는 자신의 state가 어떤값인지 어떤것을 쓰고 있는지를 알 수 없다.
  // 이것이 state를 사용하는 본질이다.
  ```

* 결론

  * `Props`

    * 읽을 수  만있다.
    * 수정되어선 안되고 수정될 수도 없다.

  * `State`

    * 비동기적으로 변화할 수 있다.
    * 수정될 때 `this.setState` 를 사용하면 쉽게 수정할 수 있다.

  * 우리 어플리케이션에선 상위컴포넌트가 app 이고 하위 컴포넌트가 Toc, Subject 등등이다. 이를 기반하여 설명보자면

    * ```markdown
      상위 컴포넌트 -> props -> 하위 컴포넌트
      ```

    * ```
      하위 컴포넌트 -> 이벤트 실행 -> 상위 컴포넌트의 state 호출 -> state 값 수정
      ```

    

* 효율적으로 바꿔보기

  * 기존의 `TOC` 클래스를 state를 사용하여 좀 더 효율적으로 바꿔보자

  ```react
  class TOC extends Component {
      render() {
          return (
              <nav>
                  <ul>
                    <li><a href="/1">HTML</a></li>
                    <li><a href="/2">CSS</a></li>
                    <li><a href="/3">JavaScript</a></li>
                  </ul>
              </nav>
          );
      }
  }
  
  															⬇️
  
  //app.js                                
  <TOC data={this.state.contents}></TOC>
    
  //toc.js
  class TOC extends Component {
      render() {
          const data = this.props.data;
          let i = 0;
          let array = [];
          while (i < data.length) {
              array.push(<li><a href={"/content/"+data[i].id}>{data[i].title}</a></li>);
              i += 1;
          }
          return (
              <nav>
                  <ul>
                      {array}
                  </ul>
              </nav>
          );
      }
  }
  ```

  * 이렇게 하므로써 좀 더 효율적으로 코드를 변경할 수 있게 되었다. 

  * 그런데 `console` 를 열어보면 에러가 뜬것을 확인할 수 있다. 

    ![image](https://user-images.githubusercontent.com/55227984/138577929-190b6acb-c19f-426e-80e2-aa25b59f8cc8.png)

    해석해 보면 각각의 `list` 는 `key` 값을 가져야 한다는 의미이다. 

    우리가 사용하기 위해서 그렇게 한것은 아니고 react 내부에서 `key` 를 사용하여 좀 더 효율적인 코드를 작성하기 위해서 그렇다.  아래와 같이 `key`를 추가하면 해결된다.

    ```react
    while (i < data.length) {
                array.push(<li key={data[i].id}><a href={"/content/"+data[i].id}>{data[i].title}</a></li>);
                i += 1;
            }
    ```

    