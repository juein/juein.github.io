---
title: "[SCSS] Guide"
categories: posts
tags:
 - CSS
date: 2020-01-02 00:00:00 -0400
---



#### [선택자]

```
div {
  color:black;
  .foo {	// 자손(descendant) 선택자
    color: black; 
  }
  > .foo {	// 자식(child) combinator
    color: black; 
  }
  + .foo {	// 인접형제(adjacent sibling) 선택자
    color: #000; 
  }
  ~ .foo {	// 일반형제(general sibling) 선택자
    color: yellow; 
  }
  & .foo {	// Sass 부모(Parent) 참조 선택자
    color: #fff; 
  }
  .foo & {	// Sass 부모(Parent) 참조 선택자
    color: red; 
  }
  &.bar {
    color: green;
  }
}
```



#### [import]

```
@import 'common.css';
@import 'common';      // scss 확장자는 생략 가능
```



#### [변수생성]

```
$imgPath : '../img/';
$white : #fff;
```



#### [믹스인]

- 생성

```
@mixin clear{
    clear: both;
    display: table;
    content : '';
}
```

- 사용

```
&:after{
	@include @clear;
}
```

- 함수처럼 매개변수 활용도 가능

```
@mixin fontInfo($size, $lh, $ls){
    font-size: $size;
    line-height: $lh;
    letter-spacing: $ls;
}

p{
	@include fontInfo(24px, 26px, -0.05em);
}
```



#### [수학 함수]

```
// 퍼센트 변경 함수
percentage(13/25) // 52%

// 반올림 함수
round(2.4) // 2

// 올림 함수
ceil(2.2) // 3

// 내림 함수
floor(2.6) // 2

// 절대값 함수
abs(-24) // 24

// 비교하여 작은것을 반환하는 함수
min(10px, 12px) // 10px

// 비교하여 큰것을 반환하는 함수
max(10px, 12px) // 12px

// 난수 함수
random(1) // 0~1


//사용
a {
  font-size: ceil(2.2) + px;
  padding: floor(2.6) + px;
  margin: max(10px,12px);
  top: min(10px,12px);
}
```



#### [문자 연산]

- 자바스크립트 처럼 `''`를 사용하지 않아도 된다.

```
cursor: poi + nter;		// pointer
```



#### [삼항 연산자]

```
$main-bg: #000;
.main {
  // $main-bg 값이 black과 같다면 #fff로 설정
  // 거짓이라면 #000으로 설정
  color: if($main-bg == black, #fff, #000);
}
```



#### [확장]

- less의 .test와 비슷한 기능을 하는 확장 (sass에서는 권장되지 않는다.)

```
.test { color: #ddd; }
.box{
  @extend .test;
}
```



#### [반복 for 문]

```
@for $i from 1 to 10 {   // to : ~ 10 미만
    .img_#{$i} {
    	background: url('img_#{$i}.jpg') no-repeat;
	}
}

@for $i from 1 through 10 {   // through : ~ 10 이하
    .img_#{$i} {
    	background: url('img_#{$i}.jpg') no-repeat;
	}
}
```

```
  $imgWidth : 100, 200, 300;
  $imgHeight : 50, 100, 200;

  @for $i from 1 through 3{
      &:nth-of-type(#{$i}){
        background-image: url('img_#{$i}.jpg');
        width: #{nth($imgWidth, $i)}px;
        height: #{nth($imgHeight, $i)}px;
      }
  }
```



#### [each]

- List(배열) 데이터 타입으로 활용 가능

```
@each $obj in aaa, bbb, ccc, ddd {
    .item-#{$obj} {
        background: url("img/#{$obj}.jpg");
    }
}

// CSS compile 결과
.item-aaa {
background-image: url("img/aaa.jpg");
}

.item-bbb {
background-image: url("img/bbb.jpg");
}

.item-ccc {
background-image: url("img/ccc.jpg");
}

.item-ddd {
background-image: url("img/ddd.jpg");
}
```

- Map 데이터 타입으로 하나의 데이터, 두개의 변수 필요

```
@each $key변수, $value변수 in 데이터 {
    // 반복 내용
}

$test-data: (
  first: aaa,
  second: bbb,
  third: ccc
);

@each $text, $data in $test-data {
  .box-#{$text} {
    background: url("/images/#{$data}.png");
  }
}

// CSS compile 결과
.box-first {
  background: url("/images/aaa.png");
}
.box-second {
  background: url("/images/bbb.png");
}
.box-third {
  background: url("/images/ccc.png");
}
```



#### [미디어 쿼리의 보간법 활용]

```
$i-phone: "only screen and (max-width: 320px)";

@media #{i-phone} {
  background: black;
}
```



#### [사용자 정의 함수]

- 반드시 return 키워드가 있어야 계산 결과를 돌려받을 수 있다.



- grid-width

```
@function grid-width($n:1) {
  // 연산 결과 반환(@return)
  @return $n * $unit-width + ($n - 1) * $gutter-width;
}

div {
  // grid-width 함수 호출 결과 값 반환(전달인자 5)
  width: grid-width(5); // 5 * 40 + (5-1) * 10 = 240px
} 
```

- px 값을 em 단위로 변경하는 함수

```
@function px2em($font_size, $base_font_size: 16) {
  @return $font_size / $base_font_size + em;
}

p {
  font-size: px2em(12, 20); // 12/20 + em = 0.6em
}
```

- 단위 제거 함수

```
@function deUnit($value) {
  @return ($value / ($value * 0 + 1))
}

p {
  font-size: PX2REM(20px);
}
```



#### [색상 관련 내장 함수]

- 채도(saturation) 변경

```
$base-color: #000;
.saturate {
  color: saturate($base-color, 20%);
}
```

- 휘도(lightness) 변경

```
$base-color: #000;
.darken {
  color: darken($base-color, 10%);
}
```

- alpha + 불투명도를 증가시킨다.(더 불투명해진다)

```
$base-color: #000;
.opacify {
  color: opacify($base-color, 0.3);
}
```

- alpha - 불투명도를 감소시킨다.(더 투명해진다)

```
$base-color: #000;
.transparentize {
  color: transparentize($base-color, 0.25);
}
```

