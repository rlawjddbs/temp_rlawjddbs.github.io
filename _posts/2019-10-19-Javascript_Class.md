---
title: Javascript Class
updated: 2019-10-19 23:32
category: javascript
---

index.html
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
dbs.js (super class)
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
람다식 함수를 활용하여 클래스화 진행

Main.js (sub class)
```javascript
(function(){
    "use strict";
    var _class = {
        _initialize : function(){
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

###### jQuery $.extend() 사용 시 
Main.js
```javascript
(function(){
    "use strict";
    var _class = {
        _initialize : function(){
            var _self = this;

            ...

        },
        
        getValue : function(key, defaultValue){
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
