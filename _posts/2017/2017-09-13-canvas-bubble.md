---
layout: default
title: canvas-bubble
categories: canvas
description: canvas-bubble
keywords: canvas, bubble
---

利用 canvas 制作的泡泡鼠标跟随效果

```
<!doctype html>
<html>
	<head>
		<title>canvas bubble</title>
		<style type="text/css">
			html, body{
				width: 100%;
				height: 100%;
				margin: 0;
				padding: 0;
			}
		</style>
	</head>
	<body>
		<canvas id="mycanvas" style="position:absolute;left:0;right:0;top:0;bottom:0;"></canvas>
		<script type="text/javascript">
			window.onload = function () {
				var mycanvas = document.getElementById("mycanvas");
				if (document.getElementsByClassName("content").length) {
					var content = document.getElementsByClassName("content")[0];
					var cWidth = content.clientWidth;
					var cHeight = content.clientHeight;
				} else {
					var cWidth = document.documentElement.scrollWidth;
					var cHeight = document.documentElement.scrollHeight;
				}
				mycanvas.width = cWidth;
				mycanvas.height = cHeight;
					var ctx = mycanvas.getContext("2d");
					//圆形类
					 function Circle(x,y,r,color){
						this.x = x;
						this.y = y;
						this.r = r;
						// 颜色的取值范围
						this.color = "rgb("+ (parseInt(Math.random() * 240 ) + 9) + ","+ (parseInt(Math.random() * 220 )+18) +",203)";
						//随机方向
						this.dx = Math.random() * 12 - 7;
						this.dy = Math.random() * 12 - 7;
						//往数组中push自己
						circleArr.push(this);
					 }
					 //渲染
					 Circle.prototype.render = function(){
						//新建一条路径
						ctx.beginPath();
						//创建一个圆
						ctx.arc(this.x, this.y, this.r, 0, Math.PI*2, true);
						//设置样式颜色
						ctx.fillStyle = this.color;
						//通过填充路径的内容区域生成实心的图形
						ctx.fill();
					 }
					 //更新
					 Circle.prototype.update = function(){
						this.x += this.dx;
						this.y += this.dy;
						this.r--;
						if(this.r < 0){
							for (var i = 0; i < circleArr.length; i++) {
								if (circleArr[i] === this) {
									circleArr.splice(i,1);
								};
							}
							return false;
						}
						return true;
					 }
					 //创建一个数组
					 var circleArr = [];
					 //鼠标移动事件
					 mycanvas.onmousemove = function(event){
						new Circle(event.clientX + window.scrollX,event.clientY + window.scrollY,30,"orange");
					 }
					 //设置定时器每20毫秒更新和渲染
					 setInterval(function(){
						ctx.clearRect(0, 0, cWidth, cHeight)
						for (var i = 0; i < circleArr.length; i++) {
							circleArr[i].update() && circleArr[i].render();
						};
					 },20);
					}       
		</script>
	</body>
</html>
```

<canvas id="mycanvas" style="position:absolute;left:0;right:0;top:0;bottom:0;"></canvas>
<script type="text/javascript">
	window.onload = function () {
		var mycanvas = document.getElementById("mycanvas");
		if (document.getElementsByClassName("content").length) {
			var content = document.getElementsByClassName("content")[0];
			var cWidth = content.clientWidth;
			var cHeight = content.clientHeight;
		} else {
			var cWidth = document.documentElement.scrollWidth;
			var cHeight = document.documentElement.scrollHeight;
		}
		mycanvas.width = cWidth;
		mycanvas.height = cHeight;
			var ctx = mycanvas.getContext("2d");
			//圆形类
			 function Circle(x,y,r,color){
				this.x = x;
				this.y = y;
				this.r = r;
				// 颜色的取值范围
				this.color = "rgb("+ (parseInt(Math.random() * 240 ) + 9) + ","+ (parseInt(Math.random() * 220 )+18) +",203)";
				//随机方向
				this.dx = Math.random() * 12 - 7;
				this.dy = Math.random() * 12 - 7;
				//往数组中push自己
				circleArr.push(this);
			 }
			 //渲染
			 Circle.prototype.render = function(){
				//新建一条路径
				ctx.beginPath();
				//创建一个圆
				ctx.arc(this.x, this.y, this.r, 0, Math.PI*2, true);
				//设置样式颜色
				ctx.fillStyle = this.color;
				//通过填充路径的内容区域生成实心的图形
				ctx.fill();
			 }
			 //更新
			 Circle.prototype.update = function(){
				this.x += this.dx;
				this.y += this.dy;
				this.r--;
				if(this.r < 0){
					for (var i = 0; i < circleArr.length; i++) {
						if (circleArr[i] === this) {
							circleArr.splice(i,1);
						};
					}
					return false;
				}
				return true;
			 }
			 //创建一个数组
			 var circleArr = [];
			 //鼠标移动事件
			 mycanvas.onmousemove = function(event){
				new Circle(event.clientX + window.scrollX,event.clientY + window.scrollY,30,"orange");
			 }
			 //设置定时器每20毫秒更新和渲染
			 setInterval(function(){
				ctx.clearRect(0, 0, cWidth, cHeight)
				for (var i = 0; i < circleArr.length; i++) {
					circleArr[i].update() && circleArr[i].render();
				};
			 },20);
			}       
</script>

