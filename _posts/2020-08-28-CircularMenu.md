---
title: JS - 원형 컨텍스트 메뉴
updated: 2020-08-28 10:38
category: javascript
---

### JS 원형 컨텍스트 메뉴
![circularContextMenu](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/200828/circular_contextmenu.gif)


```javascript
(function(){
    "use strict";

    const _class = function() {
        
        function initialize() {

            // -------------------------------
            // [1] draw & set the contextmenu
            // -------------------------------
            drawMenuComp(6);
            setMenuComp({ startAngle: 315, divCnt: 8 });

        }

        function drawMenuComp(liCnt) {

            var $compContainer = $(".circular-menu > ul");
            var html = "";
            for(var i = 0; i < liCnt; i++) {
                html += "<li></li>";
            }
            var $html = $(html);
            $compContainer.append($html);

        }

        /**
         * @param startAngle 시작 각도
         * @param divCnt 나눌 개수
         */
        function setMenuComp(option) {
            const circle = 360;
            var $comp = $(".circular-menu > ul").find("li");
            
            if (!option.startAngle) {
                option.startAngle = circle;
            }

            if (option.startAngle > circle) {
                option.startAngle -= circle;
            }
            
            if (!option.divCnt || option.divCnt < $comp.length) { 
                option.divCnt = $comp.length; 
            }
            
            var division = circle / option.divCnt;
            for(var i = 0; i < $comp.length; i++){
                option.startAngle += division;
                if(option.startAngle > circle) { option.startAngle %= circle; }
                $comp.eq(i).css({
                    "-webkit-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "-moz-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "-ms-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "-o-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                });
            }
            
        }

        return initialize();
    };

    window.cctv = $.extend(window.cctv || {}, {
        CircularContextMenu : _class
    });
    
})();
```

