---
layout: post
title: base库之拖拽
tags: chrome
datetime: 2014-04-24 10:00
pv: 40
comment: 1
share: 
---

	$().extend('drag',function(){
		for (var i = 0; i < this.nodes.length; i++) {
	 	addEvent(this.nodes[i],'mousedown',function(e){
	 		if(trim(this.innerHTML).length==0){
	 			e.preventDefault();
	 			}
	 		var _this=this;
	 		var diffX=e.clientX-_this.offsetLeft;
	 		var diffY=e.clientY-_this.offsetTop;
	 		if(typeof _this.setCapture!='undefined'){
	 			_this.setCapture();
	 		}
	 		if(e.target.tagName=='H2'){
	 			addEvent(document,'mousemove',move);
	 			addEvent(document,'mouseup',up);
	 		}else{
	 			removeEvent(document,'mousemove',move);
	 			removeEvent(document,'mouseup',up);
	 		}
	 		function move(e){
	 				var left=e.clientX-diffX;
	 				var top=e.clientY-diffY;
	 				if(left<0){
	 					left=0;
	 				}else if(left<=getScroll().left){
	 					left=getScroll().left;
	 				}else if(left>getInner().width+getScroll().left-_this.offsetWidth){
	 					left=getInner().width+getScroll().left-_this.offsetWidth;
	 				}
	 				if(top<0){
	 					top=0;
	 				}else if(top<=getScroll().top){
	 					top=getScroll().top;
	 				}else if(top>=getInner().height+getScroll().top-_this.offsetTop){
	 					top=getInner().height+getScroll().top-_this.offsetTop;
	 				}
	 				_this.style.left=left+'px';
	 				_this.style.top=top+'px';
			}
			function up(e){
				removeEvent(document,'mousemove',move);
				removeEvent(document,'mouseup',up);
				if(typeof _this.releaseCapture !='undefined'){
					_this.releaseCapture();
				}
			}
		});
	}
		return this;
	});