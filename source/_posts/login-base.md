---
layout: post
title: js库之弹出窗口
tags: chrome
datetime: 2014-04-06 18:00
pv: 40
comment: 1
share: 
---

###**让弹出的窗口居中显示**

	Base.prototype.center=function(width,height){
	
	//获取居中显示的top和left(居中显示的坐标)值,getInner()是获取浏览器的可视大小
	
	//(不同浏览器获取的值不同，有的可能包括滚动条的大小值),
	
	//height和width是弹出窗口的大小，这个不是固定值，自由设定！
	
	var top=(getInner().height-height)/2+getScroll().top;
	
	 var left=(getInner().width-width)/2+getScroll().left;
	 
	 //从存储元素的数组中取出代表窗口div的那个元素，重新设置她的位置！这样就实现了居中显示的效果
	 
	for (var i = 0; i < this.nodes.length; i++) {
	
		this.nodes[i].style.top=top+'px';
		
		this.nodes[i].style.left=left+'px';
		
	}
	
	//返回base对象，这样就能实现连缀效果
	
	return this;
	
	}
	
###**以上是具体代码实现**

	Base.prototype.resize=function(fn){
	
		//从存储元素的数组中获取元素
		
	for (var i = 0; i < this.nodes.length; i++) {
	
	//把获取到的元素一一赋值element
	
		var element=this.nodes[i];
		
		//调用window的onresize方法，当浏览器的窗口大小改变
		
		addEvent(window,'resize',function(){
		
		//重新获取浏览器的窗口大小，再次执行center方法，重新让弹出的窗口居中显示，
		
		//这样就能随着浏览器窗口的改变，让弹出窗口始终居中显示
		
			fn();
			
			//下面可能是防止弹出窗口拖动到浏览器可视窗口的外面，检查是否已经到达浏览器窗口边缘，
			
			//是就改变弹出窗口的相识位置！
			
			if(element.offsetLeft>getInner().width+getScroll().left-
			
			element.offsetWidth){
			
				element.style.left=getInner().width+getScroll().left-
				
				element.offsetWidth+'px';
				
				if(element.offsetleft<=0+getScroll().left){
				
					element.style.left=0+getScroll().left+'px';
					
				}
				
			}else if(element.offsetTop>getInner().height+getScroll().top-
			
			element.offsetHeight){
			
				element.style.top=getInner().height+getScroll().top-
				
				element.offsetHeight+'px';
				
				if(element.offsetTop<=0+getScroll().top){
				
					element.style.top=0+getScroll().top+'px';
					
				}
				
			}
			
		});
		
	}
	
	//返回base对象，这样就能实现连缀效果
	
	return this;
	
	};
	

###**通过以上两个方法，就能很完美的实现弹出窗口居中显示的效果！**