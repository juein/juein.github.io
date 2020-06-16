---
title: "[JavaScript] 반응성 (Reactivity)"
categories: posts
tags:
  - JavaScript
date: 2020-06-03 00:00:00 -0400
---

자바스크립트의 **반응성(reactivity)** 에 대해 알아보자.  


기본적으로 변수의 동작은 접근과 할당으로 되어있음   
```
var a = 1; //값 할당
a;         // 접근
a = 2;     //값 재할당
```

이 개념으로 [Object.defineProperty ()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 를 사용해보자.
Object.defineProperty () 는 객체의 속성을 재정의 (접근과 할당시 동작을 정의) 해주는 자바스크립트 내장 객체이다.   



```
Object.defineProperty(대상 객체, 객체의 속성, {
	//정의할 내용
});
```


간단하게 콘솔창에서 테스트 해보자   
```
var a = {};

Object.defineProperty(a, 'str', {
  // getter 역할을 한다. 속성에 접근했을 때 동작하는 코드를 구현
  get: function(){
    console.log('접근');
  },
  // setter 역할을 한다. 속성에 값을 할당했을 때 동작하는 코드를 구현
  set: function(setVal){
    console.log('할당', setVal);
    document.body.innerHTML = setVal;
  }
});
```

a 객체의 str 속성 값을 재정의 할때마다 값이 변하는걸 실시간으로 확인할 수 있다.

---

![코드 실행 예시 이미지 1](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-06-03-reactivity_1.png)

---

![코드 실행 예시 이미지 2](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-06-03-reactivity_2.png)

---

![코드 실행 예시 이미지 3](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-06-03-reactivity_3.png)

---


