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
</head>
<body>
    
</body>
</html>
```
  
dbs.js (super class)
```javascript
(function(){
    "use strict";

    window.o2exg = {
        version : "1.0"
    }
    var _class = {
        
        initialize : function(){
            alert("asdf");       
        }

    }

    window.dbs = _class || {};
    // window.dbs = _class || new Object();
})();
```
  
jQuery $.extend() 사용 시
```javascript
(function(){
    "use strict";
    var _class = {
        
        initialize : function(){
            alert("asdf");       
        }

    }

    $.extend(window, _class || {});
    // window.dbs = {} 으로 작성 가능
})();
```
