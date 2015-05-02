---
layout: post
title: html5时钟笔记
tags: chrome
datetime: 2014-04-04 22:00
pv: 40
comment: 10
share: 
---

####使用html5新增的canvas标签
####在js中获取canvas标签
####document.getElementById('canvas ')
####获取canvas的2d(画时钟只需要2d环境)绘图环境
####canvas.getContext('2d')
####首先绘制表盘
####设置描边的宽度
	function drawClock(){
	//创建时间对象

			var now=new Date();
		//获取当前时间小时分针秒
		var seconds=noew.getSeconds();
		var min=now.getMinutes();
		var hour=now.getHours();

		//算出小时数和分钟数(浮点数)
		hour=hour+min/60+sec/3600;
		hour=hour>12hour-12:hour;
		min=min+sec/60;	
		context.clearRect(0,0,600,600);
		context.lineWidth=10
		设置描边的颜色
	
		context.strokeStyle='red'
		##开启一个新路径
		context.beginpath()
		##画圆
		context.arc(300,300,200,0Math.PI*2,false);
		context.stroke()
		##时刻度
	for(var i=0;i<12;i++){
		//保存当前的绘图环境
		context.save();
		context.beginPath();
		//设置时针线宽
		context.lineWidth=7;
		//设置时针的颜色
		context.strokeStyle='#000';
		//坐标点重置
		context.translate(300,300);
		//围绕中心旋转指定角度
		context.rotate(Math.PI/6*i);
		//线段开始坐标
		context.moveTo(0,-170);
		//结束坐标
		context.lineTo(0,-190);
		context.stroke();
		//释放绘图环境
		context.restore();
	}
##分刻度
	for(var i=0;i<60;i++){
		//保存当前的绘图环境
		context.save();
		context.beginPath();
		//设置时针线宽
		context.lineWidth=5;
		//设置时针的颜色
		context.strokeStyle='#ccc';
		//坐标点重置
		context.translate(300,300);
		//围绕中心旋转指定角度
		context.rotate(Math.PI/30*i);
		//线段开始坐标
		context.moveTo(0,-180);
		//结束坐标
		context.lineTo(0,-190);
		context.stroke();
		//释放绘图环境
		context.restore();
	}

###画时针
###相当于新建图层
	context.save();
	context.beginPath();
	context.lineWidth=7;
	context.strokeStyle='#000';
	context.translate(300,300);
	context.rotate(Math.PI/6*3);
	context.moveTo(0,0);
	context.lineTo(0,-140);
	context.stroke();
	context.restore();

##画分针
	context.save();
	context.beginPath();
	context.lineWidth=5;
	context.strokeStyle='#000';
	context.translate(300,300);
	context.rotate(Math.PI/30);
	context.moveTo(0,0);
	context.lineTo(0,-170);
	context.stroke();
	context.restore();

##画分针
	context.save();
	context.beginPath();
	context.lineWidth=3;
	context.strokeStyle='#000';
	context.translate(300,300);
	context.rotate(Math.PI/30*3);
	context.moveTo(0,0);
	context.lineTo(0,-190);
	context.stroke();
	context.restore();

##时钟修饰
	context.beginPath();
	context.arc(250,250,7,0,Math.PI*2,false);
	context.fill();
	context.lineWidth=3;
	context.stroke();
	}
####先手动调用(定时器一秒以后才起作用);
####drawClock();

####//定时器
####setInterval(drawClock(),1000);