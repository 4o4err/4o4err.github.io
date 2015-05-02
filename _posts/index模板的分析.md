---
layout: post
title: index模板的分析
tags: chrome
datetime: 2014-04-07 11:00
pv: 40
comment: 1
share: 
---



	<!DOCTYPE html>
	<html>
 	 <head>
  		<meta charset="utf-8" />
   	 <title>Blog</title>
    	<link rel='stylesheet' href='/stylesheets/style.css' />
    	<script charset="utf-8" src="/kindeditor/kindeditor-min.js"></script>
   	 <script charset="utf-8" src="/kindeditor/lang/zh_CN.js"></script>
   	 <script type="text/javascript">
     	 var editor;
     	 KindEditor.ready(function(K){
      	  editor=K.create('textarea[name="post"]',{
        	  allowImageUpload:false,
         	 items:[
            	'fontname','fontsize','|','forecolor','hilitecolor','|','bold','underline','in	sertorderedlist','removeformat','|','image','link'
        	  ]
       	 });
      	});
    	</script>
 	 </head>
 	 <body>
  	//判断用户是否存在，存在显示下面的欢迎信息
 	 <% if(user ){ %>
   	 <div id="userInfo">
    	 <p><a href="/u/<%= user.name %>">欢迎:<%= user.name %></a></p>
   	 </div>
  	<% } %>
  	<header>
  	//在渲染模板的时候，title是制定的，这里设定的主页
  	<h1><%= title %></h1>
  	</header>
  	//搜索栏，输入关键词，回车页面显示搜索的结果，这里是模糊查找文章的标题
 	 <span><form action="/search" method="GET"><input type="text" name="keyword" 	placeholder="SEARCH" class="search" /></form></span>
  	//简单的导航栏，不同的项目实现不同的功能，
 	 //存档，列出所有发表的文章
  	//标签，列出所有文章中添加的标签
  	//友链，展示友情链接
  	//照片，选择照片添加到public目录下的images目录
 	 //登出，退出当前登录的用户
 	 //登录，登录已有的账户
  	//注册，注册一个账户，
  	<nav>
  		<span><a href="/" title="主页">主页</a></span>
   	 <span><a href="/archive" title="存档" >存档</a></span>
   	 <span><a href="/tags" title="标签" >标签</a></span>
   	 <span><a href="/links" title="友情链接" >友链</a></span>
    	<% if(user) { %>
    	<span><a href="/upload" title="upload">照片</a></span>
   	 <span><a href="/post" title="发表">发表</a></span>
    	<span><a href="/logout" title="登出">登出</a></span>
    	<% } else { %>
  		<span><a href="/login" title="登陆">登陆</a></span>
  		<span><a href="/reg" title="注册">注册</a></span>
   	 <% } %>
  	</nav>
  	<article>
  	//这里显示不同项目的错误或者成功信息
 	 <% if(success) { %>
   	 <div><%= success %></div>
  	<% } %>
  	<% if(error) { %>
    	<div><%= error %></div>
 	 <% } %>