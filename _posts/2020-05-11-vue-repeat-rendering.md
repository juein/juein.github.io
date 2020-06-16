---
title: "[Vue] 반복렌더링 & 조건적용"
categories: posts
tags:
  - Vue
date: 2020-05-011 00:00:00 -0400
---


#### 유니크 키가 없는 배열

배열 조작을 가하기엔 적합하지 않지만, 단순 출력을 위한 용도로는 문제가 없다.

```
<select>
	<option v-for="item in list">{{ item }}</option>
</select>
```

```
data: {
	list : ['리스트1', '리스트2', '리스트3']
}
```



#### 리터럴에 직접 v-for 적용

```
<p v-for="item in 3">{{ item }} 번째 P </p>
```



#### 문자열에 v-for 적용

```
<p v-for="item in text">{{ item }}</p>
```

```
data : {
	text: 'TEST'
}
```





#### 반복 렌더링

```
const data = {
  monster : [
    { id: 1, name: '슬라임', hp: 100 },
    { id: 2, name: '고블린', hp: 200 },
    { id: 3, name: '드래곤', hp: 500 }
  ],
}

const app = new Vue({
  el : '#app',
  data : data,
})
```



- v-for="**각 요소의 변수명** in **반복 대상** 배열or객체"

```
<li v-for="mob in monster">
```

- "**각 요소의 변수명**" 부분을 괄호로 감싸면 배열의 인덱스도 받을 수 있다.

```
<li v-for="(mob, index) in monster">
```

- 객체의 경우 '값, 키, 인덱스' 순으로 받을 수 있다. (순서 지켜야해)

```
<li v-for="(mob, key, index) in monster">
```

- 반복문 렌더링 시에도 요소 식별과 렌더링 부작용 방지를 위해 유니크한 key 속성을 부여해주자

```
<li v-for="(mob, idx) in monster" v-bind:key="mob.id">
```



```
<div id="app">
   <ul>
    <li v-for="(mob, idx) in monster" v-bind:key="mob.id">
      [ id : {{mob.id}} ] [ name: {{mob.name}} ]  [ HP: {{mob.hp}} ]
    </li>
  </ul>
</div>
```





#### 반복 렌더링에 조건 적용

- 클래스 조작

monster 리스트의 mob hp가 300이상이면 '강하다!' 문구를 출력해주자

```
 <li v-for="(mob, idx) in monster" v-bind:key="mob.id">
      [ id : {{mob.id}} ] [ name: {{mob.name}} ]  [ HP: {{mob.hp}} ]
      <strong v-if="mob.hp > 300"> 강하다!</strong>
</li>
```

- 출력조건

monster 리스트의 mob hp가 300보다 작은 몹만 출력하자

```
<li v-for="(mob, idx) in monster" v-bind:key="mob.id" v-if="mob.hp < 300">
      [ id : {{mob.id}} ] [ name: {{mob.name}} ]  [ HP: {{mob.hp}} ]
</li>
```



#### 리스트에 요소 추가

배열의 push나 unshift로 리스트 요소 추가를 할 수 있다.

```
<figure>
    <div>몹 추가</div>
    이름 : <input type="text" v-model="addMonsterName">
    HP : <input type="text" v-model="addMonsterHp">
    <button v-on:click="mobAdd">add</button>
</figure>
```

```
const data = {
  monster : [
    { id: 1, name: '슬라임', hp: 100 },
    { id: 2, name: '고블린', hp: 200 },
    { id: 3, name: '드래곤', hp: 500 }
  ],
  addMonsterName : '',
  addMonsterHp : 0
}

const app = new Vue({
  el : '#app',
  data : data,
  methods : {
    
    mobAdd : function(){
      //리스트 내부에서 가장 큰 id값 추출
      const max = this.monster.reduce(function(a,b){
         return a > b.id ? a : b.id;
      }, 0);
      
      this.monster.push({
        id: max + 1,
        name : this.addMonsterName,
        hp : this.addMonsterHp
      }) 
    }

  },
})
```



#### 리스트에서 요소 제거

배열 메소드인 splice로 요소를 제거할 수 있다.

```
<li v-for="(mob, idx) in monster" v-bind:key="mob.id">
      [ id : {{mob.id}} ] [ name: {{mob.name}} ]  [ HP: {{mob.hp}} ]

      <button v-on:click="mobDelete(idx)">delete</button>
</li>
```

```
methods : {
    mobDelete : function(idx){
      this.monster.splice(idx, 1);
    }
}
```



#### 리스트 요소 변경

공격 버튼 클릭시 몹의 hp가 10씩 감소되도록 적용해보자

```
<li v-for="(mob, idx) in monster" v-bind:key="mob.id">
      [ id : {{mob.id}} ] [ name: {{mob.name}} ]  [ HP: {{mob.hp}} ]

      <button v-on:click="mobAttack(idx)">attack</button>
</li>
```

```
methods : {
    mobAttack : function(idx){
      this.monster[idx].hp -= 10; // HP 10 감소
    }
}
```





<p class="codepen" data-height="772" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="xxwWqYz" style="height: 772px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="vue - basic - 반복렌더링 &amp;amp; 조건 적용">
  <span>See the Pen <a href="https://codepen.io/juein/pen/xxwWqYz">
  vue - basic - 반복렌더링 &amp; 조건 적용</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



#### 기타 요소 조작

리스트 요소에 대해서 push, splice 외에

pop, sort, reverse 등의 메소드를 이용해서 리스트 조작을 할 수도 있다.



