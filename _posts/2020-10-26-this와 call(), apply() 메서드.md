---
title: this와 call(), apply() 메서드
updated: 2019-10-26 22:27
category: JS
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
   
## call(), apply() 메서드
메서드를 호춣한 객체, 이벤트가 발생한 객체가 아니면 대부분의 this는 기본 값인 window가 될 것임(DOM에 한해서).
`call()`, `apply()`메서드는 **this를 조작하는 메서드**로서, 원하는 객체를 this로 할당하고 싶을 때 사용함.
또한 `Function.prototype`에 정의된 메서드이기 때문에 어떤 함수든 호출이 가능함.
```js
someFunction.call("this로 지정할 객체", 인자1, 인자2, ...);
someFunction.apply("this로 지정할 객체", [배열]);
```
`call()`과 `apply()` 메서드의 차이는 위와 같음. 함수를 호출할 때 this를 바꿔주는 기능은 같지만 매개변수로
인자들을 넘겨주느냐, 배열을 넘겨주느냐의 차이임.   
   
```js
var person = {
    name: "victolee",
    email: "asdf@example.com",
    birth: "0225",
    foo : function(val1, val2, val3){
        console.log(val1 + val2 + val3);
        console.log(this);
    }
}

person.foo.call(window, 3,6,9);
```
- 위의 예제는 call() 메서드를 사용하여 foo의 this 객체를 person에서 window로 변경함
- foo() 메서드를 호출하기 위해서는 person 객체에서 호출해야 하므로 원래 this는 person임
- 그러나 call() 메서드를 호출할 때 첫 번째 인자로 window 객체를 전달 했으므로 this는 window 객체로 변경됨
   
```js
var person = {
    name: "victolee",
    email: "asdf@example.com",
    birth: "0225",
    foo : function(){
        sum = arguments[0] + arguments[1] + arguments[2]

        console.log(sum);
        console.log(this);
    }
}

let arr = [3,6,9]
person.foo.apply(window, arr);
```
- 위의 예제는 apply() 함수를 호출하여 this 객체를 person 객체가 아닌 window 객체로 바꾼 코드이며, 설명은 동일함
   
## $.when() 메서드
- 제이쿼리 `when()`은 1개 이상의 함수를 비동기 방식($.ajax)으로 모두 실행하고 그 후 `done()` 
메서드를 통해 콜백 처리를 할 수 있음
- ajax의 결과를 리턴받아 처리하도록 도와줌
- 즉 `ajax`를 사용하는 경우 발생하는 `Promise` 객체를 처리할 수 도 있음   
   
```js
(function(){
    "use strict";
    var _class = {

        A : null,
        B : null,

        initA : function () {
            var _self = this;

            _self.A = 1;
        },

        initB : function () {
            var _self = this;

            _self.B = 2;
        },

        _whenSample : function () {
            var _self = this;

            $.when(_self.initA(), _self.initB()).done(function() {
                console.log(_self.A + _self.B); // 3
            });
            
        }

    }

    dbs.cmmn = dbs.cmmn || {};
    dbs.cmmn.Util = _class;
})();
```
   
- 위 코드는 js(ES5)로 구성한 class 형태의 `Util` 객체
- 객체 내부에 존재하는 `A`, `B` 필드 변수는 null이며 두 변수를 초기화하는 
`initA`, `initB` 메서드가 있음
- `_whenSample` 메서드는 제이쿼리의 `when` 메서드를 사용하여 `initA`, `initB` 메서드를 실행하고 두 메서드의 프로세스가 끝나면 
`done()` 메서드를 통해 초기화 된 `A`와 `B`의 값을 더한 결과를 출력함
> ### when()의 효용성
> - 단일한 ajax를 호출하는 경우보다는 여러개의 ajax 콜이 필요한 웹 사이트에서 단계적으로, 또는 동시에 결과를 처리하기 위해서 많이 쓰임
>   - 순차적으로 ajax 결과에 따라 다른 ajax를 수행하는 경우
>   - 여러 ajax의 결과가 함께 필요한 경우
> - when()이 필요한 이유는 ajax는 비동기식 통신이므로 모든 ajax의 처리가 언제될 지 알 수 없기 때문이기도 함

## when.apply()
위 예제 소스를 통해 알 수 있는 제이쿼리 when() 메서드의 사용법은 다음과 같음
```js
$.when(ajax1, ajax2, ...).done(function() {
    ...
})
```
- 여기서 인자로 사용된 `ajax1`, `ajax2`, ... 등의 arguments를 나열하지 않고 배열에 모아서 처리를 할 수 있는데, 앞서 알아본 `apply()` 함수를 사용하면 가능함
   
```js
var deferreds = []; // 1
// var deferreds = [ajax1, ajax2];
deferreds.push(ajax1); // 2
deferreds.push(ajax2);

$.when.apply(null, deferreds).done(function() { // 3
    ...
})
```
1. ajax 비동기 통신 함수를 모아놓을 배열 `deferreds`를 선언함
2. deferreds 배열에 함수를 push 함
3. $.when 메서드 뒤에 apply 함수를 붙여 인자를 배열로 처리
   
### when.apply()로 결과 받기
```js
function ajax1() {
    var deferred = $.Deferred();

    // ajax1의 비동기 통신 프로세스...
    
    return deferred.promise();
}

function ajax2() {
    var deferred = $.Deferred();

    // ajax2의 비동기 통신 프로세스...
    
    return deferred.promise();
}

$.when.apply(null, deferreds).done(function(r1, r2) {
    ...
})
```