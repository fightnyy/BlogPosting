# React 3장

* **HTML** 은 기본적으로 설정되는 값이 있다. 

  * 예를 들어 `a` 태크같은 경우 는 `Reload` 가 기본값이다.

  * 이를 방지하기 위해선 `preventDefault()` 를 사용하면된다.

  * 또한 파이썬의 `pdb.set_trace()` 처럼 크롬에서는 `debugger;` 라는 키워드를 제공한다.

    ```react
    <h1><a href="/" onClick={function (e) {
                           console.log(e);
                           e.preventDefault();
          								 debugger;
                        }}>{this.state.subject.title}</a></h1>
    ```

    



# Event

* 우리는 이제 `WEB` 이라는 곳을 클릭했을때 기본적이 `welcome` 페이지가 바뀌었으면 좋겠다.

  * 정확히는 `mode` 의 값을 바꿔주려고 한다. 이렇게 하기 위해선 `Event` 라는것이 필요하다.
  * 따라서 수정해보자

  ```react
  <h1>
    <a href="/"
       onClick={function (e) {
       e.preventDefault();
       this.state.mode = "read";
       }}>
       {this.state.subject.title}
    /a>
  </h1>
  ```

* 이러면 바로 오류가 발생한다. 바로`component` 안에서의  `this` 가 뭘 가르키는지 모르기 때문이다. 또한 `react` docs에는 바로 이렇게 바꾸지 말고 `setStatus` 라는것을 사용하라고 알려준다.

  * 따라서 `this`를 bind 해주자
  * `setStatus` 를 사용하자

  ```react
  <h1>
    <a href="/"
       onClick={function (e) {
       e.preventDefault();
       this.setState({
         mode:'welcome'
       });
       }.bind(this)}>
       {this.state.subject.title}
    /a>
  </h1>
  ```



## bind

* 우리가 `render` 함수에서 `this` 를 `console.log` 를 사용해서 출력해보면 해당 `component` 를 가르키는 것을 알 수 있다.

* 하지만 `onClick` 함수 안에서 `this` 를 `console.log` 로 출력해보면 `undefined` 가 나온것을 알 수 있다.

* 따라서 우리가 할 일은 `onClick` 에서도 `this` 가 해당 컴포턴트가 되게 하는 일이다. 

  ```javascript
  let obj = {name:'hello'};
  
  function bindTest(){
    console.log(this.name);
  }
  
  bindTest();// undefined 가 출력됨
  
  
  let bindTest2 = bindTest().bind(obj);
  
  bindTest2(); //hello 가 출력됨.
  ```

   

## setState

* `constructor` 안에서는 `setState` 사용안해야 한다. 그냥 값을 원래하던 대로 `=` 을 사용해서 바꾸면 된다.
  * 실제로 값이 안바뀐것은 아니다 분명히 값은 바뀌었다. 하지만 `react` 만 모르고 있을뿐....... 
* 하지만 `constructor` 외부에서는 `=` 를 사용해서 바꾸면 `react` 가 알아차릴 수가 없다. 
* 따라서   `setState` 함수를 사용해줘야 한다.





## `Component`에 이벤트 만들기

* 그럼 이제  `react` 에서 기본 `Component` 를 만들어보자

  ```react
  //app.js
  <Subject
    title={this.state.subject.title}
    sub={this.state.subject.sub}
    onChangePage={function () { // 이벤트 명은 아무거나 내가 지정해도 된다.
      this.setState({mode: 'welcome'})
      }.bind(this)}> //여기서의 this는 App Component 를 가르킨다.
  </Subject>
  
  
  //Subject.js
  class Subject extends Component {
      render() {
          return (
              <header>
                  <h1><a href="/" onClick={
                      function (e) {
                          e.preventDefault();
                          this.props.onChangePage();
                      }.bind(this) //여기서의 this 는 Subject의 this를 가르킨다.
                  }>{this.props.title}</a></h1>
                  {this.props.sub}
              </header>
          );
      }
  }
  ```

  



## 꿀팁

* bind함수에 (this, 12, 34) 이런식으로 this뒤에 새로운 파라미터를 넣는다면 기존함수에 새로운 파라미터를 추가해주는것과 동일하다.

  ```react
  function (name, id, e) {
    e.preventDefault();
    console.log(name) // youngyun
    console.log(id) // 25
    this.props.onChangePage();
  }.bind(this, "youngyun", 25)
  ```

* `concat` 함수를 사용하면 원본 배열을 변환하지 않고 값을 추가할 수 있다.

  ```javascript
  let arr = [1,2]
  arr.push(3)
  
  let arr2 = [1, 2];
  
  let result = arr2.concat(3);
  
  console.log(arr) //[1,2,3]
  console.log(arr2) // [1,2]
  console.log(result) // [1,2,3]
  ```

  

* `setState` 를 사용할 때는 성능측면에서 기본 값에 어떤 값을 추가하는 것보다 어떤 값으로 완전히 바꿔버리는 것이 더 좋은 해결책이다.

  ```javascript
  this.state.contents.push({
    id:this.max_content_id, titile:_title, desc:_desc
  }) //ANTI PATTERN
  
  
  let _contents = this.state.contents.concat(
  {
    id:this.max_content_id, title:_title, desc:_desc
  }
  )
  
  this.setState({
    contents: _contents
  }) //Good Pattern
  ```

* 그럼 왜 원본을 수정하면 안되는 것일까? 결론은 최적화 시에 해당 내용이 매우 치명적인 역할로 쓰일 수 있기 때문이다.

  * 위 함수`(shouldComponentUpdate)`를 한번 살펴보자

    ```javascript
    class TOC extends Component{
      shouldComponentUpdate(newProps, nextState){
        console.log('===>TOC shouldComponentUdpate');
        return true;
      }
      
      render(){
        console.log('===> TOC render')
        ...
      }
    }
    ```

    * 해당 문제의 경우 함수의 경우 우선 `true` 일 경우엔 `render` 함수가 호출된다. 하지만 `false` 경우에는 `render` 함수가 호출되지 않는 것을 볼 수 있다.

    * 그럼 위의 매개변수들은 어떤 것을 표시하는 것일까?

      * `newProps` 의 경우 새롭게 변경된 `Props` 를 나타낸다. 이와 비교하기 위해선 `this.props` 를 보면 더욱 잘 비교를 할 수 있다.


  * 그래서 이게 최적화랑 어떤 연관이 있다는 것일까?


    * 우리가 알다시피 `state` 또는 `props` 의 값이 변경된다면 해당 컴포넌트와 그 하위 컴포넌트의 `render` 함수가 다시호출된다.

      
      * 그런데 어떤 하위 컴포넌트의 구성요소는 바뀌지 않았는데 상위컴포넌트의 `state` 값이 변경되었다면 어쩔 수 없이 하위 컴포넌트의 `render` 함수가 다시 호출된다. 따라서 이를 해결하기 위하여 아래와 같이 코드를 구성할 수 있을것이다.
      
        ```javascript
        shouldComponentUpdate(newProps, newState){
        if(this.props.data === newProps.data){
          return false;
        }
        return true;
        }
        
        render(){
          ...
        }
        ```
        
      * 하지만 만약 내가 `state` 안에 있는 값을 직접 수정해버린다면 `this.props` 와 `newProps` 의 차이가 없게 된다.
      
      
        *  같은 값이 들어와지기 때문에 `render` 가 항상 실행된다.
        * 따라서 새로운 컴포넌트를 만들어서 써야하는것이다.

  * `Array.from(배열)` 을 사용하면 내용은 같지만 다른 배열을 가질 수 있다.

    ```javascript
    let a = [1,2]
    let b = Array.from(a)
    console.log(a, b, a===b) //[1,2] , [1,2] , false
    ```

    

  * 이와 비슷하게 복제된 객체를 갖고 싶다면 `Object.assign(초기값, 추가할 값)` 을 사용하면 된다.

    ```javascript
    let a = {name:'egoing'}
    let b = Object.assign({}, a);
    console.log(a, b, a===b) //{name: 'egoing'} , {name: 'egoing'} false
    
    let c = Object.assign({left: 1, right: 2}, a)
    console.log(c) //{left: 1, right: 2, name:'egoing'}
    
    ```

  * `JS` 의 문제는 객체나 배열등의 명령어가 때로는 immutable 하고 때로는 mutable 하다는 것이다. 이를 해결하기 위해선 `immutableJS` 와 같은 라이브러리를 사용하면 모든 배열 및 객체들을 `Immutable` 하게 사용할 수 있다.

  

  