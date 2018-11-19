##### introduction

- Why use ReactJS?
  - just javascript
  - composition 
    - 리액트 구조는 요소별, 컴포넌트 별로 나누어서 작업할 수 있음
  - unidirectional dataflow
    - UI에서 데이터 변경이 일어나지 않음
- React is not a framework, but UI library


- creating react components with JSX
  - JSX : 리액트 컴포넌트를 만들 때 사용하는 언어, 규칙 심플

##### Components and Props

- Components는 각각의 functions와 methods를 가진다
  - All components should have a render function 
- ReactDOM is the way we rendered react on a website
  - ReactDOM renders one component

##### Component Lifecycle 

- Render 실행 순서
  - componentWillMount() -> render() -> componentDidMount()
- Update 실행 순서
  - componentWillReceiveProps() -> shouldComponentUpdate() -> componentWillUpdate() -> render() -> componentDidUpdate() 
- componentWillReceiveProps() : 컴포넌트가 새로운 Props를 받음
- shouldComponentUpdate() 는 이전의 Props와 새로운 Props를 비교 후, 두 Props가 서로 다른 경우  true를 의미
- 활용
  - componentWillUpdate() 내에 spinner 혹은 로딩중 메시지를 띄울 수 있음



##### State

- **State** is just an object that is inside of every react component
- Whenever your state changes inside of the component, the function "render" will happen again
- state를 변경하고 싶은 경우, `this.state.greeting`에 직접 접근하는 게 아니라, `setState({})`를 활용해야 한다 
  - 이 때, state가 변경되어 업데이트 될 때마다 render가 실행된다

##### Stateless Functional Components

- 모든 컴포넌트가 state를 가지는 것은 아니다
- dumb components don't have render(), state, class.

##### AJAX

##### Finishing Up

##### Building for Production





