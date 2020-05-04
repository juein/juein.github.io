---
title: "[JavaScript] 이벤트 위임 (event delegation)"
categories: posts
tags:
  - JavaScript
date: 2020-05-04 00:00:00 -0400
---

### JS 이벤트 위임



![Cheat Sheet](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2020-05-04-jsed_1.png)

```
<div class="button-area">
  <button type="button" class="btn">
    <i class="fas fa-arrow-alt-circle-left"></i>
    <span>left</span>
  </button>
  <button type="button" class="btn">
    <i class="fas fa-arrow-alt-circle-right"></i>
    <span>right</span>
  </button>
</div>
```



위와 같은 DOM구조에서 각 버튼 클릭시 이벤트가 발생하길 원할 때



```

const btns = document.querySelectorAll('.btn');

function btnClick(){
  console.log(this);
}

for(let i = 0; i < btns.length; i++){
  btns[i].addEventListener("click", btnClick);
}

//for문이 싫다면
btns[0].addEventListener("click", btnClick);
btns[1].addEventListener("click", btnClick);

...

```



위 코드처럼 각 버튼에 이벤트리스너를 달아주면 된다.

하지만 위 처럼 버튼이 조금밖에 없는 경우라면 별 차이가 없겠지만 

버튼이 수십개가 될 경우, addEventListener가 많아지면 페이지 성능이 저하된다.  

고로 위와같은 방법보단 이벤트 위임을 사용하자

이벤트 위임시 버튼이 아닌 버튼의 부모 태그에 이벤트 리스너를 달아주고 

각 버튼에 data-value값을 넣어주자.



```
<div class="button-area">
  <button type="button" class="btn" data-value="1">
    <i class="fas fa-arrow-alt-circle-left"></i>
    <span>left</span>
  </button>
  <button type="button" class="btn" data-value="2">
    <i class="fas fa-arrow-alt-circle-right"></i>
    <span>right</span>
  </button>
</div>
```


```
const area = document.querySelector('.button-area');

function areaClick(e){
    //이벤트 객체
    //console.log(e); 

    //이벤트 핸들러에서는 this와 currentTarget이 같은것(이벤트를 호출한 객체, 즉 .button-area )을 가리킨다.
    //console.log(e.currentTarget); 

    //현재타겟 (버튼 외 이미지나 텍스트를 읽음)
    //console.log(e.target);    

    //data-value읽기
    console.log(e.target.getAttribute('data-value'));
}

area.addEventListener("click", areaClick);
```

위 소스대로라면 data-value 값이 셋팅되어있는 button을 클릭시에는 문제가 없지만

data-value가 셋팅되어있지 않으면 값을 읽어오지 못한다. (button안에있는 i, span 태그 클릭시 null출력)




- 방안 1. button 안의 자식 태그에도 data-value 속성을 기재한다 (비추천)

```
<div class="button-area">
  <button type="button" class="btn" data-value="1">
    <i class="fas fa-arrow-alt-circle-left" data-value="1"></i>
    <span data-value="1">left</span>
  </button>
  <button type="button" class="btn" data-value="2">
    <i class="fas fa-arrow-alt-circle-right" data-value="2"></i>
    <span data-value="2">right</span>
  </button>
</div>
```

- 방안 2. CSS 속성을 사용해서 자식 태그 클릭시 스크립트가 읽지 못하게 한다.

```
i, span{
  pointer-events: none;
}
```

- 방안 3. javascript code로 해결

```
function areaClick(e){
 
  let d = e.target;

  //클릭한 DOM에 button에 있는 .btn 클래스가 없으면 예외처리. DOM탐색을 반복해야하니 반복문으로 체크하자
  while(!d.classList.contains('btn')){

    d = d.parentNode; //부모의 노드를 위임
    
    // 이벤트가 걸린 .button-area 클릭시에도 .btn클래스가 없으니 트리구조를 타고 올라가다 에러가 난다. 최상위 body태그까지 탐색될 경우 예외처리하자.
    
    if(d.nodeName == 'BODY'){ //현재 노드가 body면 이벤트 무반응 처리
      d = null;
      return;
    }

  }

  console.log(d.dataset.value);

}
```


상황에 따라 방안 2, 3을 적절히 사용하자


<p class="codepen" data-height="523" data-theme-id="dark" data-default-tab="js,result" data-user="juein" data-slug-hash="gOaXKQd" style="height: 523px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="js 이벤트 위임">
  <span>See the Pen <a href="https://codepen.io/juein/pen/gOaXKQd">
  js 이벤트 위임</a> by juein (<a href="https://codepen.io/juein">@juein</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


