---
title: "[React] Styled-Components"
categories: posts
tags:
 - React
date: 2020-04-17 00:00:00 -0400
---

### 스타일 컴포넌트
※ 버전에 따라 사용법이 조금 다르다보니 공식문서를 참고하는 편이 가장 좋다.
[공식문서](https://styled-components.com/docs) : https://styled-components.com/docs


- 스타일 컴포넌트 렌더


```
import styled, { css } from 'styled-components';
```



#### 1. 기본 사용

```
const Container = styled.div`
  background: #ffeaa7;
`;

function Test() {
  return (
    <>
        <Container></Container>
    </>
  );
}
```



#### 2. SASS(SCSS)의 중첩 사용과 props 사용

```
function Test() {
  return (
    <>
        <Container>
            <Button green>green</Button>
            <Button className="fs20">pink</Button>
        </Container>
    </>
  );
}

const Button = styled.button`
  width: 100px;
  height: 50px;
  border-radius: 10px;
  border:0;
  outline: none;
  background: ${props => props.green ? '#00cec9' : '#ff7675'};
  &.fs20{
    font-size: 20px;
  }
`;
```



#### 3. 공통스타일 적용


`기존 injectGlobal이 v4부터 createGlobalStyle 로 대체되었다.`
- styled components는 해당 컴포넌트에만 스타일이 적용되는데 
injectGlobal, createGlobalStyle 사용시 공통으로 적용될 스타일을 지정할 수 있다.
ex) GlobalStyle 로 list-style:none 적용

```
function Test() {
  return (
    <>
      <GlobalStyle />
    </>
  );
}

const GlobalStyle  = createGlobalStyle`
  body {
    margin:0;
    padding:0;
    button, a{
      display: block;
      margin: 2px;
    }
  }
  ol,ul{
    list-style:none
  }
`;
```


#### 4. 확장 기능 (extend)
- 이미 선언된 버튼의 스타일을 확장

```
// 확장(extend)
const ButtonTypeExtend = styled(Button)`
  color: #fff;
  width: 200px;
`;

//extend && tag변경
const AtagBtn = styled(Button.withComponent("a"))`
  font-size: 1.5em;
  color: #fff;
  text-decoration: none;
  padding: 10px;
`;
```



#### 5. 인풋태그 속성에 따른 설정
```
const InputButton = styled.input.attrs(props => ({
  type: "button",
}))`
  width: 100px;
  height: 100px;
  border-radius: 50%;
  outline: none;
  border-width: 0;
`;
```



#### 6. 애니메이션 적용
```
// 애니메이션 키프레임 등록
const rotate = keyframes`
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
`;


const Rotation = styled.div`
  outline: 1px solid ${props => props.red ? "#ff6600" : "#000"};
  width: 100px;
  height: 10px;
  display: inline-block;

  ${props => {
    if(props.red){
      return css `animation: ${rotate} 2s linear infinite `;
    }else {
      return css `animation: ${rotate} 2s linear infinite reverse`;
    }
  }};
`;

const AppointDuration = styled(Rotation)`
  ${props => {
    return css `animation-duration : ${props.duration}s;`; 
  }};
`;

```



<p class="codepen" data-height="500" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="vYEwMvo" style="height: 500px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="React Styled-Components">
  <span>See the Pen <a href="https://codepen.io/juein/pen/vYEwMvo">
  React Styled-Components</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>