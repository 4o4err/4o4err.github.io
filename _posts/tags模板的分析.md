---
layout: post
title: tags模板的分析
tags: chrome
datetime: 2014-04-07 11:00
pv: 40
comment: 1
share: 
---


	//导入header模板
	
		<%- include header %>
		
	//从数据中返回的数据放在posts中，调用foreach函数可以得到posts中包含的项目，
	
	title、name、time、head、comments（文章标题，作者名字、发表时间、作者头像以及文章留言等信
	息）
		<% posts.forEach(function(post,index){ %>
		
	//点击标题链接，跳转到文章页面，
	
	（跳转的规则在u/:name/:title在index.js中实现app.get('/
	
	u/:name/:title',function(req,res))）
	
			<p><h2><a href="/u/<%= post.name %>/<%= post.time.minute %>/<%= 
			
			post.title%>">
			
				<%= post.title %></a></h2>
				
	//点击作者头像，跳转到作者页面（跳转的规则在u/:name在index.js中实现app.get('/
	
	u/:name',function(req,res))）
	
				<a href="/u/<%= post.name %>"><img src="<%= post.head %>" 
				
				class="r_head"></a>
				
			</p>
			
		<p class="info">
		
	//点击作者名字，跳转到作者页面（跳转的规则在u/:name在index.js中实现app.get('/
	
	u/:name',function(req,res))）
	
			作者:<a href="/u/<%= post.name%>"><%=post.name %></a>|
			
	//文章发表时间
	
			时间:<%= post.time.minute %></a>|
			
	//文章的评论
	
			评论:<%= post.comments.length %></a><br />
			
	//显示发表文章时加的标签，
	
		点击某个标签跳转到添加这个标签的文章页面（跳转的规则在index.js中实现app.get('/
		
		tags/:tag',function(req,res))）
		
			<% post.tags.forEach(function(tag,index){ %>
			
				<% if(tag){ %>
				
				<a class="tag" href="/tags/<%= tag %>"><%= tag %></a>
				
				<% } %>
				
			<% }) %>
			
		
		</p>
		
	//显示文章的详细内容
	
		<p><%- post.post %></p>
		
	//显示文章被阅读的数量
	
		<p class="info">阅读：<%= post.pv %></p>
		
		<% }) %>
		
	//引入实现翻页效果的模板
	
		<%- include pageing %>
		
	//也如footer模板
	
	<%- include footer %>