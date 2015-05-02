---
layout: post
title: js库之一些兼容函数的实现
tags: chrome
datetime: 2014-04-07 11:00
pv: 40
comment: 1
share: 
---

#####多浏览器的兼容是很烦人的，当时当IE6还依然活在一些人的脑海中的时候，当公司还在看重对ie6的兼容的时候，当xp都退役了，ie11都出来了，ie6还没有被抛弃，这说明微软的系统做的有多不成功，win8一个嘅开始按钮怎么没了，关机按钮都需要百度谷歌才能找到，有人会认真看微软的提示吗？
#####很少吧，你他妈的赶快给我显示桌面，提示个鸟啊！我艹，桌面呢！这是win8？裤子都脱了，你给我显示这个，磁块就这么不受欢迎吗？非的费劲找传统界面的入口，微软的理念很好，并且实现的很好，无奈用户被xp和win7洗脑严重不领情啊，这他妈的就是一坨翔，草泥马问候你xx！
#####win8在我看来很好用，速度安全性易用性都甩了xp好几条街，可是微软忘了，用户想要的是什么，兼容性！我一个游戏玩的好好的，当了wen8一打开就崩溃各种提示，微软你们考虑过处女座的感受吗？你还让文艺处女座活吗？你直接x掉程序还提示个鸟说应用程序已崩溃，要不要等待或者强制关闭，就连我这个狮子座都受不了，提示能文艺点吗？能简单点吗？
#####微软能不能解决兼容问题，新系统不再有兼容问题，用户特别在意这个，运行个程序就崩溃，再说什么易用速度安全美观，那都是扯犊子！用户不关心这个，只关心我喜欢的游戏能不喜欢的应用能不能完美运行。
#####微软你们能不能站在用户的角度开发系统，你们是给用户用的，大部分还是小白用户，就是极客也不可能怀着极大的包容心去爬你们挖的坑！

#####扯的好爽，接下来进入正题，关于ie和w3c浏览器的兼容问题的解决方案

	function getInner(){
	
		//判断widnow的innerWidth属性，undefined说明是ie不支持这个属性，
		
		//ie支持document.documentElement.clientWidth属性，火狐支持这个属性
		
		//w3c都支持
		
		//然后根据各自支持的属性获取width和height的值(浏览器窗口的高和宽))
		
		if(typeof window.innerWidth!='undefined'){
		
			return {
			
				width:window.innerWidth,
				
				height:window.innerHeight
				
			}
			
		}else{
		
			return {
			
				width:document.documentElement.clientWidth, 
				
				height:document.documentElement.clientHeight
				
			}
		}
	}
	
	
#获取形式的兼容实现

	function getStyle(element,attr){
	
	//ie支持element.currentStyle[attr]
	
	//w3c支持window.getComputedStyle
	
		if(typeof window.getComputedStyle!='undefined'){
		
			return window.getComputedStyle(element,null)[attr];
			
		}else if(typeof element.currentStyle!='undefined'){
		
			return element.currentStyle[attr];
			
		}
	}
	
	
#跨浏览器或link规则


	//第num个link规则，通过那个link链接进来的css文件
	
	sheet=document.styleSheets[num]
	
	//selectorText选择那个元素的id、class或者标签名,
	
	//cssText要设置的样式内容,
	
	//position要添加到选择的css文件中的那个位置
	
	function insertRule(sheet,selectorText,cssText,position){
	
		//sheet.insertRule是w3c支持的属性，删除则是sheet.deleteRule
		
		if (typeof sheet.insertRule!='undefined') {
		
			sheet.insertRule(selectorText+'{'+cssText+'}',position);
			
		}else if(typeof sheet.addRule!='undefined'){
		
			//sheet.addRule是ie支持的属性,删除则是sheet.removeRule
			
			sheet.addRule(selectorText,cssText,position);
			
		} 
	}
	
	function removeRule(sheet,index){
	
		//w3c支持这个属性
		
		if(typeof sheet.deleteRule!='undefined'){
		
			sheet.deleteRule(index);
			
		//IE支持这个属性
		
		}else if(typeof sheet.removeRule!='undefined'){
		
			sheet.removeRule(index);
			
		}
	}
	
	//在base库中使用上面兼容函数
	
	Base.prototype.addRule=function(numm,selectorText,cssText,position){
	
	//第num个link规则，通过那个link链接进来的css文件
	
		sheet=document.styleSheets[num];
		
		//向html中添加规则
		
		insertRule(sheet,selectorText,cssText,position);
		
		return this;
		
	}
	
	Base.prototype.removeRule=function(numm,index){
	
	//第num个link规则，通过那个link链接进来的css文件
	
		sheet=document.styleSheets[num];
		
		//从html中删除规则
		
		removeRule(sheet,index)
		
		return this;
		
	}