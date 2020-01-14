---
title: "[React] Hook, State"
categories: posts
tags:
 - React
date: 2020-01-14 00:00:00 -0400
---


### 클래스 컴포넌트와 함수 컴포넌트

- 리액트 컴포넌트 에서 props 데이터만 다룬다면 함수형 컴포넌트를 사용 할 수 있다.
- Hook 도입 이전(react v16.8미만) 에는 컴포넌트에서 state, 생명주기(life cycle)를 다루려면 반드시 클래스 문법을 사용 해야했다.  





**함수 컴포넌트(function component)**

```
function Test() {
  return (
      <div>test</div>
  );
}

//또는
const Test = () => {
  return (
      <div>test</div>
  );
}
```





**클래스 컴포넌트(class component)**

```
class Test extends Component {
  render() {
    return (
       <div>test</div>
    );
  }
}
```





개념적으로 React 컴포넌트는 함수형에 더 가깝다고 한다.

친절한 리액트 개발진은 클래스 문법을 잘 모르는 사람을 위해 Hook을 도입해줬다.

**Hook은 Class없이 React의 기능들을 사용하는 방법** 이다.





### Hook

- Hook 소개 : [https://ko.reactjs.org/docs/hooks-intro.html](https://ko.reactjs.org/docs/hooks-intro.html)
- 컴포넌트의 state 를 사용할 수 있는 useState 함수가, 생명주기를 다루기 위한 useEffect 함수가 추가되었다. 그 외 커스텀 Hook도 생성 가능. (보통 네이밍이 use~ 로 시작된다.)

- Hook은 필수 사용이 아니다. 기존 컴포넌트와의 호환성도 100% 이며, Hook의 도입을 위해 기존 코드를 다시 작성할 필요가 없다.
- Hook는 **클래스 안에서 동작하지 않는다.**
- Hook 사용시 [여기의 두가지 규칙](https://ko.reactjs.org/docs/hooks-rules.html)을 준수해야 한다. 





### State

- props는 부모 컴포너트가 값을 설정된 값을 컴포넌트 자신이 읽기 전용으로만 사용 할 수 있었는데, state는 컴포넌트 자체적으로 지닌 값으로 내부에서 값을 읽고, 변경할 수 있다.
- state의 값은 직접 변경이 불가능하고, setState() 메소드를 사용해서만 변경할 수 있다.



**클래스 컴포넌트에서 state 선언과 렌더**

```
// 리액트의 컴포넌트 클래스를 상속
class TestState extends React.Component {
    // 초기 state는 constructor 메서드에서 정의해야 한다.
    constructor(props) {
        super(props);
        // state 값 설정
        this.state = {
            test: 1
        };
    }
    render() {
        return (
            <>
            //state.test 값 읽기
            <div>origin test state = {this.state.test}</div>
            
            <button type="button" onClick={() => 
            		// setState로 state값을 변경
                    this.setState({  
                        test : this.state.test + 1 
                    })
                }>
                plus
            </button>
            
             <button type="button" onClick={() => 
            		// setState로 state값을 변경
                    this.setState({  
                        test : this.state.test - 1 
                    })
                }>
                minus
            </button>
            </>
        )

    }
}
```





**Hook 을 사용한 state 선언과 렌더**

```
// useState Hook을 컴포넌트에 호출
import React, {useState} from 'react';

function TestState() {
    //함수 컴포넌트는 this를 가질 수 없기때문에 this.state 대신 useState 를 사용한다.
    // state 변수와, 해당 변수를 갱신할 수 있는 함수 두개를 선언한다.
    const [test, setTest] = useState(1);
    return (
        <>
        // state 값 읽기
        <div>origin test state = {test}</div>
        
        // 선언된 setTest 로 state 값 변경
        <button type="button" onClick = {() => setTest(test + 1)}>
            plus
        </button>
        
        <button type="button" onClick = {() => setTest(test - 1)}>
            minus
        </button>
        </>
    );
}
```





<p class="codepen" data-height="454" data-theme-id="default" data-default-tab="js,result" data-user="juein" data-slug-hash="abzKOQO" style="height: 454px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Reack State">
  <span>See the Pen <a href="https://codepen.io/juein/pen/abzKOQO">
  Reack State</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



