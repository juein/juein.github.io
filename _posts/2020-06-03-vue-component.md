---
title: "[Vue] 컴포넌트"
categories: posts
tags:
  - Vue
date: 2020-06-03 00:00:00 -0400
---

## 컴포넌트
화면의 영역을 쪼개서 재사용 할 수 있는 형태로 만드는 기능(웹 화면을 헤더, 푸터, 컨텐츠영역, 네비 등으로 분할해서 화면을 구성한다고 생각하자)

#### 전역 컴포넌트
먼저 전역 컴포넌트를 등록 해보자.    

Vue.component('컴포넌트 이름', 컴포넌트 내용); 으로 등록 할 수 있다.
- js
```
Vue.component('global-component', {   // 컴포넌트 이름은 케밥 형식이 아니여도 된다.
  template: '<p>전역 컨텐츠의 내용이 요기잉네</p>'
});

const app = new Vue({
  el : '#app'
});
```

- html
```
<div id="app">
  <global-component></global-component>  // 컴포넌트 이름과 같은 태그를 생성
</div>
```


![전역 컴포넌트 렌더링 결과](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-05-06-vue-component_1.png)
<small>전역 컴포넌트 렌더링 결과</small>

#### 지역 컴포넌트

지역 컴포넌트도 등록해보자. 
전역컴포넌트랑 똑같은 방식인데 Vue 인스턴스 안에 넣어주면 된다.    

**※ 전역 컴포넌트는 Vue.`component` 이고, 지역 컴포넌트는 `components` 다. 복수형에 주의하자**

- js
```
const app = new Vue({
  el : '#app',
  components: {
    // '컴포넌트 이름' : 컴포넌트 내용
    'local-component' : {
      template: '<p>지역 컴포넌트 내용</p>'
    }
  }
});
```
- html
```
<div id="app">
  <local-component></local-component>
</div>
```

![지역 컴포넌트 렌더링 결과](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-05-06-vue-component_2.png)
<small>지역 컴포넌트 렌더링 결과</small>

#### 지역 컴포넌트의 유효 범위

지역 컴포넌트의 유효범위는 해당 컴포넌트가 등록된 인스턴스 까지다.
여러개의 뷰 인스턴스가 있을 경우 차이를 느낄 수 있다. 
뷰 인스턴스를 하나 더 만들어서 확인해보자.

- js
```
const app2 = new Vue({
  el : '#app2',
  components: {
    'local-component' : {
      template: '<p>이름은 같지만 다른 지역 컴포넌트</p>'
    }
  }
});
```

- html
```
<div id="app2">
  <global-component></global-component>
  <local-component></local-component>
</div>
```

![컴포넌트 범위 테스트](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-05-06-vue-component_3.png)
<small>컴포넌트 범위 테스트</small>

유효범위가 이렇다보니 일반적인 개발 작업을 할 때는 지역 컴포넌트를 활용하고   
플러그인이나 라이브러리를 개발할 때 전역 컴포넌트를 사용하게 된다.


<p class="codepen" data-height="649" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="eYJYWzY" style="height: 649px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="vue - Component">
  <span>See the Pen <a href="https://codepen.io/juein/pen/eYJYWzY">
  vue - Component</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



## 컴포넌트 통신

지역 컴포넌트 뿐 아니라, 모든 컴포넌트는 유효범위가 있다. 
그리고 <span id="docLink">**서로 다른 컴포넌트**</span>에서는 각각의 컴포넌트에서 정의된 **데이터를 직접 참조하지 못한다.**   


```
const firstComponent = {
  template: '<p>첫번째 컴포넌트에서 number : {{number}}</p>',
  data: function(){
    return{
      number : 10
    }
  }
}

const secondComponent = {
  template: '<p>두번째 컴포넌트에서 number : {{number}}</p>', 
  data: function(){
    return{
      number : 0 // 없으면 에러난다
    }
  }
}

const app = new Vue({
  el: '#app',
  components: {
    'first-component' : firstComponent,
    'second-component': secondComponent
  }
});
```

<p class="codepen" data-height="619" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="dyGGxoZ" style="height: 619px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="vue - Component-2">
  <span>See the Pen <a href="https://codepen.io/juein/pen/dyGGxoZ">
  vue - Component-2</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



이러한 컴포넌트의 유효범위는 불편해 보이지만 필요한 부분이다.

컴포넌트 내에서의 데이터가 모든 영역에서 공유될 경우 

특정 컴포넌트의 변화로 인한 데이터 흐름을 추적하기 어려워지기 때문. 

(규모가 큰 프로젝트면 디버깅의 헬이..)

이렇기에 뷰 컴포넌트의 데이터 전달 방법의 규칙은

위에서 아래로, **상위(부모) 컴포넌트 -> 하위(자식)** 컴포넌트로 데이터를 전달하는 **props**방식이 있고, 

**하위(자식) 컴포넌트 -> 상위(부모)** 컴포넌트로의 데이터 전달은 **event**를 발생시키는 방법이 있다.    



### 상위, 하위 컴포넌트의 관계

※ 컴포넌트 간의 부모자식 관계는 별도로 등록하는것은 아니며 
컴포넌트 등록시 Vue 인스턴스 자체가 상위(부모) 컴포넌트가 된다. 
또는 템플릿에서 다른 컴포넌트를 사용하면 부모 자식 관계가 만들어진다.


```
// 자식 컴포넌트
const childComponent = {
};

// 부모 컴포넌트 (Root 컴포넌트)
const app = new Vue({
  el : '#app',
  components: {
    'child-component': childComponent
  }
});
```

뷰 개발자도구에서 부모 자식 관계를 한 눈에 확인 가능

![뷰 개발자 도구에서 확인 한 컴포넌트의 부모자식 관계](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-06-03-vue-component_1.png)


또는 템플릿에서 컴포넌트를 사용하는 경우의 예

```
// 자식 컴포넌트
Vue.component('parent-component', {
    template: '<div> <child-component></child-component> </div>'
});

// 자식의 자식 컴포넌트
Vue.component('child-component', {
    template: '<div> 123 </div>'
})

// 부모 컴포넌트 (Root 컴포넌트)
const app = new Vue({
    el : '#app',
});
```

![뷰 개발자 도구에서 확인 한 컴포넌트의 부모자식 관계](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-06-03-vue-component_2.png)


### props
컴포넌트의 부모 자식 관계에서 부모 -> 자식 에게 데이터를 전달하는 **props**의 사용법부터 알아보자.


1. 부모 컴포넌트에서 데이터와 자식 컴포넌트 등록
```
const app = new Vue({
  el : '#app',
  data: {
    parentData : '부모의 데이터를 자식 컴포넌트에서 호출하자'
  },
  components: {
    'child-component': childComponent
  }
});
```

2. html 단에서 프롭스 속성명 / 상위 컴포넌트 이름 매칭

```
<div id="app">
  <!-- v-bind:프롭스 속성 이름 = "상위 컴포넌트 데이터 이름"-->
  <child-component v-bind:propsdata="parentData"></child-component>
</div>
```

3. 자식 컴포넌트에서 props 속성으로 데이터 전달받기

```
//자식 컴포넌트
const childComponent = {
  props: ['propsdata'],
  template: '<p>{{propsdata}}</p>'
}
```



이렇게 props속성으로 받아온 데이터는 부모 컴포넌트의 데이터가 변경될 때 

자식 컴포넌트에서의 데이터도 같이 변경된다. (reactivity, 반응성으로 되어있다.)



### event

자식 -> 부모에게 데이터를 전달할때는 **event**를 발생시켜 값을 전달한다. 
이벤트 발생과 수신은 **$emit()** 과 **v-on** 속성으로 구현한다.

1. 자식 컴포넌트에서 이벤트 등록 (클릭 이벤트로 테스트 해보자)

```
const childComponent2 = {
  template: '<input v-on:click="childClick" type="button" value="버튼"/>'
}
```

2. 자식 컴포넌트에서 $emit 으로 컴포넌트에 연결되어 있는 이벤트를 명시적으로 실행시키기 (jquery의 trigger랑 비슷함)
```
  methods: {
    childClick: function(){
      this.$emit('child-event'); //이벤트 발생
    }
  }
```

3. html에서 child-event 이벤트가 발생하면 parentEvent를 호출 하도록 예약
```
<div id="app">
  <!-- v-on:하위 컴포넌트 이벤트 이름="상위 컴포넌트 메소드 이름"-->
  <child-component2 v-on:child-event="parentMethod"></child-component2>
</div>

```

4. 부모 컴포넌트에서 이벤트 잡아서 실행
```
const app = new Vue({
  el : '#app',
  components: {
    'child-component2': childComponent2
  },
  methods: {
    parentMethod: function(){
      console.log('부모 컴포넌트에서 이벤트 실행');
    }
  }
});
```


자식 컴포넌트에서 등록 된 버튼 클릭시, 부모 컴포넌트에서 지정된 함수가 실행되는걸 확인 할 수 있다.


<p class="codepen" data-height="669" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="OJMPvaL" style="height: 669px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="vue - props &amp;amp; event">
  <span>See the Pen <a href="https://codepen.io/juein/pen/OJMPvaL">
  vue - props &amp; event</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### 이벤트 버스

이쯤 와서 재확인 해보자. 위에서 언급한 [서로 다른 컴포넌트](#docLink) 의 관계는 부모 자식 관계가 아니다.

![뷰 개발자 도구에서 확인 한 컴포넌트의 관계](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-06-03-vue-component_3.png)


이처럼 부모 자식 관계가 아닌 컴포넌트는 props와 event로 데이터를 주고 받을 수 없기에 **이벤트 버스** 기능을 사용한다.

1. 이벤트 버스 전용 인스턴스 만들기
```
var bus = new Vue();
```

2. 값을 넘기고자 하는 컴포넌트에서 이벤트 발생시 **eventBus** 인스턴스를 이용한 **$emit** 으로 발생할 이벤트 명과 데이터를 전달.

```
const firstComponent = {
    template: '<p>첫번째 컴포넌트에서 number : {{number}} <button v-on:click="busEvent">값 넘기기</button></p>',
    data: function(){
        return{
          number : 10
        }
    },
    methods:{
        busEvent: function(){
          // 이벤트버스 인스턴스.$emit('발생할 이벤트 명', 전달 데이터);
          bus.$emit('numberUpdate', this.number);
        }
    }
}
```

3. 값을 받고자 하는 컴포넌트에서 **eventBus** 인스턴스의 이벤트를 **$on** 으로 수신
```
const secondComponent = {
    template: '<p>두번째 컴포넌트에서 number : {{number}}</p>', 
    created: function(){
          var self = this;
          // 이벤트버스 인스턴스.$on('수신 이벤트 명', 수신 데이터)
          // 이벤트 수신시 함수 처리도 가능
          bus.$on('numberUpdate', function(value){
          self.number = value;
        });
    },
    data: function(){
        return {
          number : 0
        }
    }
}
```

<p class="codepen" data-height="832" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="qBbZxzz" style="height: 832px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="vue -eventBus">
  <span>See the Pen <a href="https://codepen.io/juein/pen/qBbZxzz">
  vue -eventBus</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


이벤트 버스는 편리하지만, 많이 사용하면 인스턴스와 이벤트가 꼬여서 코드가 복잡해진다.   

