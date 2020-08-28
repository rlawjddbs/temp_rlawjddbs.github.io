---
title: JS - 원형 컨텍스트 메뉴
updated: 2020-08-28 10:38
category: javascript
---
**요약** : transform으로 회전 후 회전시킨 방향으로 밀어내고 원래대로 회전

### JS 원형 컨텍스트 메뉴
> 1. cctv 버튼 클릭 시 원형 메뉴가 나타나야 함
> 2. 원형 메뉴는 메뉴의 개수 및 각도를 자유롭게 조절할 수 있어야 함
> 3. 테스트를 위한 cctv 버튼 요소의 clone 생성 후 임의 배치

   
![circularContextMenu](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/200828/circular_contextmenu.gif)

#### Main.js
```javascript
(function(){
    "use strict";
    
    const _class = function() {

        function initialize() {
            initEvent();
        }
        
        function initEvent() {
            
            // cctv btn event
            $("button.btnCctv").off("click").on("click", function(e){
                e.stopPropagation();

                var _self = $(this);

                $(".circular-menu li").removeClass("active");
                $("button.btnCctv").removeClass("active");
                _self.addClass("active");

                var _self = $(this);
                var w = _self.width() / 2;
                var h = _self.height() / 2;

                var cw = $(".circular-menu").width() / 2;
                var ch = $(".circular-menu").height() / 2;

                var thisOffset = $(this).offset();

                // var x = thisOffset.left + _self.width() - 35;
                // var y = thisOffset.top + _self.height() / 2 - 35;
                var x = thisOffset.left - cw + w;
                var y = thisOffset.top - ch + h; 
                
                $(".circular-menu").css({
                    left: x,
                    top: y,
                    display: "block"
                });

                $(".circular-menu li").off("click").on("click", function(){
                    console.log($(this).index());
                    _self.removeClass("active");
                })
                
                $(document.body).one("click", function(e){
                    if(!$(e.target).hasClass("btnCctv")){
                        $(".circular-menu").hide();
                    }
                    _self.removeClass("active");
                });
                
            });

        }

        return initialize();
    }

    
    window.cmmn = $.extend(window.cmmn || {}, {
        Main : _class
    });
})();
```

#### CircularContextMenu.js
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

#### html
```html
...
<body>
    <!-- [begin] cctv button -->
    <button class="btnCctv" style="position:absolute; top:200px; left:200px;"></button>
    <!-- [end] cctv button -->

    <!-- [begin] circular-menu -->
    <div class="circular-menu" style="display:none;">
        <ul>
        </ul>
    </div>
    <!-- [end] circular-menu -->

</body>
...
```

#### CSS 
```css
/* contextmenu */
.circular-menu{position:absolute; width: 70px; height: 70px; z-index:50;}
.circular-menu ul{width: 100%; height:100%; margin:0; padding:0; border:2px solid #ffa500; border-radius:50px; box-sizing:border-box; background-color:rgba(255, 255, 255, .82);}
.circular-menu li{width:20px; height:20px; position:absolute; border-radius:10px; background:#fff; box-sizing:border-box; border:2px solid #ff0000; list-style:none; left:25px; top:25px; cursor:pointer;}

/* button */
button{cursor: pointer;}
.btnCctv{width:20px; height:21px; border: none; background: url(../images/bg_cctv_cam.png) no-repeat center center; outline:none; padding:0; margin:0; z-index:45;}
.btnCctv.active{z-index:55;}
```

