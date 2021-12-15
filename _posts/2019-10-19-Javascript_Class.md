---
title: Javascript Class
updated: 2019-10-19 23:32
category: JS
---

#### index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="common/js/dbs.js"></script>
    <script src="common/js/Main.js"></script>
</head>
<body>
    
</body>
</html>
```

#### dbs.js (super class)
```javascript
(function(){
    "use strict";

    window.dbs = {
        version : "1.0"
    };

    dbs.defined = function(value) {
        return value !== undefined && value !== null;
    }

    dbs.isEmpty = function(value) {
		return value === undefined || value == null || value == "" || $.isEmptyObject(value);
	}

	dbs.isNotEmpty = function(value) {
		return !dbs.isEmpty(value);
	}

    window.dbs = _class || {};
    // window.dbs = _class || new Object();
})();
```
- 람다식 함수를 활용하여 클래스화 진행


### Object 방식
#### Main.js (sub class)
```javascript
(function() {
    "use strict";
    var _class = { // Object style
        _initialize : function() {
            var _self = this;

            ...

        },
        
        getValue : function(key, defaultValue){
            return key != undefined && key != null && key != "" ? key : defaultValue;
        },

        ...
    } // class

    dbs.cmmn = dbs.cmmn || {};
    dbs.cmmn.Main = _class;

})();
```
- `Object` 방식
- `instance` 생성 안해도 `dbs.cmmn.Main`로 원하는 속성 접근 가능
- `Object.assign()` 으로 객체 합치는 것 가능(`polyfill` 필요)

#### jQuery $.extend() 사용 시 
Main.js
```javascript
(function() {
    "use strict";
    var _class = {
        _initialize : function() {
            var _self = this;

            ...

        },
        
        getValue : function(key, defaultValue) {
            return key != undefined && key != null && key != "" ? key : defaultValue;
        },

        ...
    } // class

    dbs.cmmn = $.extend(dbs.cmmn || {}, {
        Main : _class
    });
    // window.dbs = {} 으로 작성 가능
})();
```
- `Object.assign()` 대신 `$.extend()` 사용

### 함수 방식
```javascript
(function() {
    "use strict";
    var _class = function() { // function style

        // ---------------------
        // Outter Function
        // ---------------------
        function outterFunction() {
            ...
        }

        // ---------------------
        // Innter Function
        // ---------------------
        let initialize = function() { // 내부 함수 선언 방식1
            var _self = this;

            ...

        }
        
        function getValue(key, defaultValue) { // 내부 함수 선언 방식2
            return key != undefined && key != null && key != "" ? key : defaultValue;
        }

        initialize();

        // ---------------------
        // Outter
        // ---------------------
        this.outterFunction = outterFunction; // 이 방식은 인스턴스를 생성해야 유효한 위치에 등록됨

        ...
    } // class

    dbs.cmmn = dbs.cmmn || {};
    dbs.cmmn.Main = _class;

})();
```
- 함수 방식으로 1회성으로 사용할 목적으로 클래스를 만들어 호출할 수 있음
- `외부 함수` 등록 시 반드시 해당 클래스의 `인스턴스`를 만들어야 올바르게 접근 가능
    - `인스턴스` 없이 `dbs.cmmn.Main()`을 호출할 경우 `dbs.cmmn.outterFunction()` 으로 외부 함수 위치가 알맞지 않은 구조...에 위치한 것을 확인할 수 있음