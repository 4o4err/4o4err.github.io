---
layout: post
title: js库之遮蔽罩
tags: chrome
datetime: 2014-04-07 10:00
pv: 40
comment: 1
share: 
---

#####在弹出窗口登陆的时候，页面其他的地方不能进行交互操作，只能在当前弹出的窗口进行相应的操作，实现这一功能需要占满当前浏览器窗口的div元素，并且设置样式z-index足够大，z-index和页面层次有关，值越大层次越靠前，当值比其他元素都大，这个元素就在其他元素的最前面，当这个元素占满窗口，页面的其他部分就不能响应用户的操作了，只能操作在页面最前面的元素。
#####遮蔽罩就是这个原理，她的z-index值只比弹出的窗口的值小一点点，这样就实现只能操作弹出窗口的需求，没有她，可能页面显示不止一个的弹出窗口，这时候页面的用户体验就是一坨热乎乎的翔，浏览者心中会有成千上万只草泥马飘过！
##下面是具体实现过程！
#####***首先在html代码中添加一个div元素，设置样式***



	screen{
	
		//相对定位，她必须铺满整个浏览器窗口
		
		position: absolute;
		
		top: 0;
		
		left: 0;
		
		background-color: #666;
		
		z-index: 9998;
		
		//设置透明程度，让页面其他内容能被看到，到不能操作
		//ie设置背景透明不一样：filter:alpha(opacity=30)
		
		opacity: 0.3;
		
		opacity: 0;
		
		//默认是不显示，她时随着弹出窗口的弹出而显示的！
		
		display: none;
	}

##***样式设置完毕，接下来就由js来具体实现了，在页面中添加一个登录或者其他按钮，点击以后会弹出窗口并相识遮蔽罩，***
	var login=$('#login');
	
	//首先让弹出窗口居中显示(center函数)，当浏览器窗口改变大小(resize函数),
	
	//弹窗再次调用center函数让窗口居中显示，这样用户体验会很好！
	
	 login.center(400,300).resize(function(){
	 
	 	//login.center(400,300);
	 	
	 	//判断弹窗当前是不是显示，显示(display:block),
	 	
	 	if(login.css('display')=='block'){
	 	
	 	//再让遮蔽罩显示并改变大小(lock()函数，后面会说她的功能！)
	 	
	 		$('#screen').lock();
	 		
	 	}
	 	
	 });
	 
	 //当点击登录按钮，弹出登录窗口，
	 
	 $('#header .login').click(function(){
	 
	 //并让弹窗居中显示
	 
	 	login.center(400,300)
	 	
	 	//默认弹窗是不显示的，也没必要显示出来
	 	
	 	login.css('display','block');
	 	
	 	//遮蔽罩调用lock()函数，显示并铺满整个屏幕，并加入动画效果(动画效果后面再说)
	 	
	 	$('#screen').lock().animate({
	 	
	 		'attr':'o',
	 		
	 		'target':65,
	 		
	 		'step':10,
	 		
	 		't':30
	 		
	 	});
	 	
	 });
	 
	 //当关闭弹窗
	 
	 $('#login .close').click(function(){
	 
	 //弹窗关闭
	 
	 	login.css('display','none');
	 	
	 	//再遮蔽罩消失之前会有动画效果(动画效果后面再说)，然后再消失(调用unlock())
	 	
	 	$('#screen').animate({
	 	
	 		'attr':'o',
	 		
	 		'target':0,
	 		
	 		'step':10,
	 		
	 		't':30,
	 		
	 		fn:function(){
	 		
	 			$('#screen').unlock();
	 			
	 		}
	 		
	 	});
	 	
	 });
	 
	 
##***下面说说lock和unlock函数是怎么实现的***


#lock函数实现
	Base.prototype.lock=function(){
	
		//for循环的目的是获取代表遮蔽罩的元素
		
		for (var i = 0; i < this.nodes.length; i++) {
		
			fixedScroll.top=getScroll().top;
			
			fixedScroll.left=getScroll().left;
			
			//获取left值
			
			var left=getScroll().left;
			
			//让遮蔽罩显示
			
			this.nodes[i].style.display='block';
			
			//当浏览器的窗口大小改变的时候，重新设置遮蔽罩的width和height值
			
			this.nodes[i].style.width=getInner().width+getScroll().left+'px';
			
			this.nodes[i].style.height=getInner().height+getScroll().top+'px';
			
			//this.nodes[i].style.display='block';
			
			//给遮蔽罩添加mousedown和mouseup事件，执行predef事件，这个后面细说
			
			addEvent(this.nodes[i],'mousedown',predef);
			
			addEvent(this.nodes[i],'mouseup',predef);
			
			addEvent(this.nodes[i],'selectstart',predef);
			
			//overflow属性忘记了，可能和滚动条有关！
			
			document.documentElement.style.overflow='hidden';
			
			addEvent(window,'scroll',fixedScroll);
			
		}
		
		return this;
		
	}
	


#unlock函数实现

	Base.prototype.unlock=function(){
	
		//for循环的目的是获取代表遮蔽罩的元素
		
		for (var i = 0; i < this.nodes.length; i++) {
		
		//遮蔽罩隐藏
		
			this.nodes[i].style.display='none';
			
			//宽和高都设为0
			
			this.nodes[i].style.width=0;
			
			this.nodes[i].style.height=0;
			//移除在lock函数中的事件
			removeEvent(window,'scroll',fixedScroll);
			
			removeEvent(this.nodes[i],'mousedown',predef);
			
			removeEvent(this.nodes[i],'mouseup',predef);
			
			removeEvent(this.nodes[i],'selectstart',predef);
			
		}
		
		return this;
		
	}