---
title: Javascript Key Event
updated: 2020-07-03 09:46
category: javascript
---

<a href="https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/0703/result.gif" style="border-bottom:0;" target="_new">![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/0703/result.gif)</a>  
  
JS 데이터 목록 화살표 키 Event

```javascript
(function() {
	"use strict";
	
	var _class = function() {
		// ------------------------------
		// var
		// ------------------------------
		var _self = this;

		var shiftDown = false;
		var ctrlDown = false;
		var upDown = false;
		var downDown = false;
		
		// ------------------------------
		// outer function
		// ------------------------------


		// ------------------------------
		// inner function
		// ------------------------------
		function initialize() {
			initPanel();
			initEvent();
		}

		function initPanel() {
			var $lnbList = $("div.lnb-list");
			$.each($lnbList, function(i, v){
				$(v).attr("tabindex", i);
			});
			
		} 

		function initEvent() {
			// ------------------------------------------
			// [Ctrl + A] Key Event & [Delete] Key Event
			// ------------------------------------------
			$("div.lnb-list").on("keydown", function(e){
				var $this = $(this);
				
				// shift
				if(e.which === 16 || e.keyCode === 16 || event.metaKey){
					_self.shiftDown = true;
				}
				// ctrl
				if(e.which === 17 || e.keyCode === 17 || event.metaKey){
					_self.ctrlDown = true;
				}
				// shift + up
				if(_self.shiftDown === true && e.keyCode === 38){
					_self.upDown = true;
					$liNode = $("div.lnb-list").find("li.active");
					
					if($liNode.length === 0){
						$liNode = $(this).find("li").first();
						$liNode.addClass("active");
					} else if ($liNode.length >= 1 && _self.downDown === null || _self.downDown === undefined || _self.downDown === false ) {
						if($liNode.prev().prop("tagName") === "LI") {
							$.extend($liNode, $liNode.prev());
						}
						$liNode.addClass("active");
					} else if ( $liNode.length > 1 && _self.downDown === true ){
						$liNode.last().removeClass("active");
						$liNode = $(this).find("li.active");
					} else if ( $liNode.length === 1 && _self.downDown === true ){
						_self.downDown = false;
						if($liNode.prev().prop("tagName") === "LI") {
							$.extend($liNode, $liNode.prev());
						}
						$liNode.addClass("active");
					}
					scrollToSelection();
					return;
				}
				// shift + down
				if(_self.shiftDown === true && e.keyCode === 40){
					_self.downDown = true;
					$liNode = $(this).find("li.active");
					
					if($liNode.length === 0){
						$liNode = $("div.lnb-list").find("li").first();
						$liNode.addClass("active");
					} else if ($liNode.length >= 1 && _self.upDown === null || _self.upDown === undefined || _self.upDown === false ) {
						if($liNode.next().prop("tagName") === "LI") {
							$.extend($liNode, $liNode.next());
						}
						$liNode.addClass("active");
					} else if ( $liNode.length > 1 && _self.upDown === true ){
						$liNode.first().removeClass("active");
						$liNode = $(this).find("li.active");
					} else if ( $liNode.length === 1 && _self.upDown === true ){
						_self.upDown = false;
						if($liNode.next().prop("tagName") === "LI") {
							$.extend($liNode, $liNode.next());
						}
						$liNode.addClass("active");
					}
					scrollToSelection(true);
					return;
				}
				// ctrl + A 
				if(_self.ctrlDown === true && e.keyCode === 65){
					e.preventDefault();
					$(this).find("li").addClass("active");
					return;
				}
				// delete
				if(e.which === 46 || e.keyCode === 46){
					var $liNode = $(this).find("li.active");
					var sheetList = o2sheet.sheet.SheetController.getSheetList();
					var currentIndex = 0;
					for(var i = 0; i < $liNode.length; i++){
						for(var j = 0; j < sheetList.length; j++){
							if ($liNode.eq(i).data("DATA_INFO").DATASET_ID === sheetList[j].getDataMeta().DATASET_ID){
								currentIndex = j - 1 < 0 ? 0 : j - 1;
								o2sheet.sheet.SheetController.closeSheet(sheetList[j].getDataMeta().DATASET_ID, sheetList[j].getDataSe());
								o2sheet.sheet.SheetController.activeSheet( o2sheet.sheet.SheetController.getSheetList()[currentIndex] );
							}
						}
					}
					$liNode.remove();
					return;
				}
				// up
				if(e.which === 38 || e.keyCode === 38){
					e.preventDefault();
					$liNode = $("div.lnb-list").find("li.active");
					
					if($liNode.length === 0){
						$liNode = $("div.lnb-list").find("li").eq(0);
						$liNode.addClass("active");
					} else if ($liNode.length === 1) {
						$("div.lnb-list").find("li").removeClass("active");
						$liNode = $liNode.prev().prop("tagName") === "LI" ? $liNode.prev() : $this.find("li").first();
						$liNode.addClass("active");
					} else {
						$("div.lnb-list").find("li").removeClass("active");
						$liNode = $liNode.first();
						$liNode.addClass("active");
					}
					scrollToSelection();
					return;
				}
				// down
				if(e.which === 40 || e.keyCode === 40){
					e.preventDefault();
					$liNode = $("div.lnb-list").find("li.active");
					
					if($liNode.length === 0) {
						$liNode = $("div.lnb-list").find("li").eq(0);
						$liNode.addClass("active");
					} else if ($liNode.length === 1) {
						$("div.lnb-list").find("li").removeClass("active");
						$liNode = $liNode.next().prop("tagName") === "LI" ? $liNode.next() : $this.find("li").last();
						$liNode.addClass("active");
					} else {
						$("div.lnb-list").find("li").removeClass("active");
						$liNode = $liNode.last();
						$liNode.addClass("active");
					}
					scrollToSelection();
					return;
				}
			}).on("keyup", function(e){ 
				if(e.which == 16 || e.keyCode == 16){
					_self.shiftDown = false;
				}
				if(e.which == 17 || e.keyCode == 17){
					_self.ctrlDown = false;
				}
			});

		} // initEvent

		// 선택된 데이터가 스크롤 영역 밖에 있을 경우 스크롤
		// @Param reverse : boolean - true 설정 시 선택된 데이터 중 마지막 데이터를 기준으로 스크롤 (기본값은 가장 첫번째 데이터 기준으로 스크롤) 
		function scrollToSelection(reverse) {
			
			var $parent = $("#lnb").find("li.active").parents(".lnb-list");
			var $selected = $("#lnb").find("li.active");
			var selectedHeight = $selected.eq(0).outerHeight();
			
			var start = $parent.scrollTop() - selectedHeight/2 ;			// scrollTop - 시작점 
			var end = start + $parent.outerHeight() - selectedHeight/2;		// scrollTop + 스크롤요소 높이 - 종료점
			
			var referenceIdx = reverse === true ? $selected.last().index() : $selected.first().index(); 
			
			if( (referenceIdx * selectedHeight) < start || (referenceIdx * selectedHeight) > end ){
				var scrollValue = selectedHeight * (referenceIdx-2);
				if ($parent.outerHeight() < (selectedHeight * 2 + selectedHeight / 2) ) {
					scrollValue = selectedHeight * $selected.eq(0).index();
				}
				$parent.scrollTop( scrollValue );
			}

		} // scrollToSelection
	} // _class

	// ------------------------------
	// init
	// ------------------------------
	return initialize();

	dbs.data = $.extend(dbs.data || {}, {
		DataList : _class
	});

})();
```

