---
title: css常用代码
categories: 'css'
tags: 
    - "代码"
    - "代码片段"
---

### 垂直居中

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>上下垂直居中 在线演示 DIVCSS5</title>
<style>
#main {position: absolute;width:400px;height:200px;left:50%;top:50%;
margin-left:-200px;margin-top:-100px;border:1px solid #00F}
</style>
<body>
<div id="main">DIV水平居中和上下垂直居中</div>
</body>
</html>
```



### 超长隐藏

长度必须设置

```css
width:200px;
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```



### 溢出给滚动条

```css
overflow-x: hidden;
overflow-y: aotu;
```



### div新增蒙板

```css
background-color:#fff;filter:Alpha(Opacity=60);opacity:0.6;
```



### 居中绝对定位

```css
position: absolute;
top:50%;
left: 50%;
transform: translate(-50%,-50%);
```



### input绑定回车事件

```
vue2: @keyup.enter.native="方法名称"
vue3: v-on:keyup.enter="方法名称"
```

