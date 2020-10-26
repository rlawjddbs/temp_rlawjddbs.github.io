---
title: this와 call(), apply() 메서드
updated: 2019-10-26 22:27
category: javascript
---
### 예제 1
```js
var person = {
    name: "victolee",
    email: "asdf@example.com",
    birth: "0225",
    foo : function(){
        console.log(this); 
    }
}

person.foo()
```
- 위 예제에서 `foo()` 함수를 실행하기 위해서는 person 객체의 foo 프로퍼티(속성)을 참조해야 함
- 즉, `foo()` 함수를 호출한 것은 `person` 객체이므로, `this`는 person 객체를 가르킴

### 예제 2
```js
var x = 10;
function foo() {
    this.x = 20;
    x = 30;

    console.log(x);
    console.log(this.x);
    console.log(window.x);
}
```
- 이번에는 `foo()` 함수를 어떤 객체가 호출한 것이 아니기 때문에, this는 특정 객체를 가르키고 있지 않음
- 일반 함수일 경우에는 `window` 객체를 this로 갖고 있음(`엄격모드`에서는 undefined)
> 사실 예제에서 x를 선언한 부분은
> - x = 10 
> - foo() 함수 내에서 this.x = 20
> - foo() 함수 내에서 x = 30
   
window.x = 10   
window.x = 20   
window.x = 30   
으로 작성한 것과 똑같기 때문에, 정답은 모두 30이 출력 됨
   
### 예제 3
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script
    src="https://code.jquery.com/jquery-3.2.1.min.js"
    integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
    crossorigin="anonymous"></script>
</head>
<body>
<button class="btn" value="foo">버튼</button>
</body>

<script>
    $(".btn").click(function(){
        console.log(this)
    })
</script>

</html>
```
- 위의 예제는 jQuery 코드임
- button을 클릭 했을 시 jQuery의 click 이벤트가 실행되는데, 이 경우에 jQuery의 셀렉터인 class="btn"인 요소가 this가 됨
- 즉, 이벤트가 발생했을 때는 이벤트가 발생한 객체가 this임
   
위의 내용을 정리하면 this가 생성되는 경우는 다음과 같음
- 객체가 메서드를 호출할 경우, 메서드를 호출한 객체가 this임
- 일반 함수인 경우, 브라우저 상에서 window가 this임
- 이벤트가 발생한 경우, 이벤트를 발생한 객체가 this임