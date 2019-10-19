---
title: Javascript Class
updated: 2019-10-19 23:32
category: javascript
---

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="common/js/ClassTest.js"></script>
    <script type="text/javascript">
        dbs.initialize();
    </script>
</head>
<body>
    
</body>
</html>
```

```javascript
var dbs;

(function(){
    "use strict";
    var _class = {
        
        initialize : function(){
            alert("asdf");       
        }

    }

    dbs = _class;
})();
```
  
jQuery $.extend() 사용 시
```javascript
var dbs;

(function(){
    "use strict";
    var _class = {
        
        initialize : function(){
            alert("asdf");       
        }

    }

    $.extend(dbs, _class);
})();
```
