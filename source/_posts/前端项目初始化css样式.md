---
title: 前端项目初始化css样式
categories: 'css'
tags: 
    - "初始化"
    - "默认样式"
thumbnail: "https://cdn.pixabay.com/photo/2015/12/04/14/05/code-1076536_1280.jpg"
---

> 前端项目再初创的时候，会清空掉原浏览器的样式。这里给出一些常规的清除样式，和一些美化样式。仅供参考

### common.scss
```
#app {
	height: 100vh;
	background: #f0f2f5;
	position: relative;
}
.el-button--small {
	padding: 7px 12px !important;
	font-size: 13px !important;
	--el-button-size: 27px !important;
}
.search-box {
	display: flex;
	align-items: center;
	background: #ffffff;
	flex-wrap: wrap;
	padding: 17px 20px 7px 20px;
	.s-item {
		margin-right: 10px;
		border-radius: 2px;
		flex-grow: 0;
		flex-shrink: 0;
		margin-bottom: 10px;
	}
}
.mt-1 {
	margin-top: 10px;
}
.w-60 {
	width: 60px !important;
}
.w-80 {
	width: 80px !important;
}
.w-100 {
	width: 100px !important;
}
.w-120 {
	width: 120px !important;
}
.w-150 {
	width: 150px !important;
}
.w-200 {
	width: 200px !important;
}
.w-250 {
	width: 250px !important;
}
.w-300 {
	width: 300px !important;
}
.w-400 {
	width: 400px !important;
}
::v-deep .el-button--primary {
	background: #0078ff;
}
.main-container {
	height: calc(100vh - 144px);
	display: flex;
	flex-direction: column;
}
.status-success {
	color: #00b389;
}
.el-button--primary {
	--el-button-border-color: #0078ff;
}
:root {
	--el-color-primary: #0078ff;
	--el-color-success: #0fba57;
	--el-color-danger: #e84c3d;
	--el-dialog-padding-primary: 0px;
	--el-border-radius-base: 3px;
}
.el-input {
	--el-input-focus-border: #c0c4cc;
}
.form-dialog {
	font-size: 20px;
	.el-button--large {
		--el-button-size: 38px;
	}
	> .el-dialog__header {
		font-weight: 600;
		color: #333;
		padding-top: 10px;
		background: #f5f5f5;
		padding: 12px 0 12px 30px;
		border-bottom: 1px solid #ebebeb;
		margin-right: 0;
		.el-dialog__title {
			font-size: 15px;
		}
		.el-dialog__headerbtn {
			top: 0;
		}
	}
	&.header-center {
		.el-dialog__header {
			padding: 12px 0 12px 0;
			display: flex;
			justify-content: center;
		}
	}
	> .el-dialog__body {
		padding-right: 60px;
		padding-bottom: 20px;
	}
	> .el-dialog__footer {
		display: flex;
		justify-content: center;
		border-radius: 0 0 4px 4px;
		padding: 12px 20px 12px 0;
		background: #f5f5f5;
	}
}
.base-form {
	.el-cascader,
	.el-select,
	.el-input,
	.el-input__wrapper {
		width: 100% !important;
	}
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
	-webkit-appearance: none !important;
}
input[type='number'] {
	-moz-appearance: textfield !important;
}

```

### element-ui.scss
```
.el-dialog__header {
  margin-right: 0px !important;
  background-color: #f5f5f5;
}

.el-dialog__body {
  padding: 20px !important;
}

.el-drawer__header {
  background-color: #f5f5f5;
  padding: 20px !important;
  margin-bottom: 0px;
  color: #333333;
  font-weight: 600;
  margin-bottom: 20px !important;
}
```

### index.scss
```
@import './common.scss';
@import './reset.scss';
@import './element-ui.scss';
```

### reset.scss
```
* {
	-webkit-tap-highlight-color: transparent;
}
*,
:after,
:before {
	-webkit-box-sizing: border-box;
	box-sizing: border-box;
}
::-moz-selection {
	background: #6190e8;
	color: #fff;
}
::selection {
	background: #6190e8;
	color: #fff;
}

html,
body,
div,
header,
footer,
hr,
nav,
p,
code,
pre,
section,
td,
th,
form,
h1,
h2,
h3,
h4,
h5,
h6,
input,
textarea,
button,
select,
article,
aside,
blockquote,
dd,
details,
fieldset,
figcaption,
figure,
hgroup,
legend,
menu,
audio,
canvas,
video,
dl,
dt,
ol,
ul,
li {
	padding: 0;
	margin: 0;
}
html {
	line-height: 1.15;
	font-size: 14px;
	-webkit-text-size-adjust: 100%;
	-ms-text-size-adjust: 100%;
}
body {
	font-family: 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
	-ms-text-size-adjust: 100%;
	-webkit-text-size-adjust: 100%;
	-webkit-font-smoothing: antialiased;
	min-height: 100vh;
	overflow: auto;
}
p {
	font-size: inherit;
	line-height: 24px;
}
h1,
h2,
h3,
h4,
h5,
h6 {
	color: inherit;
	font-weight: bolder;
	margin-bottom: 20px;
}
h1,
.h1 {
	font-size: 1.75rem;
}
h2,
.h2 {
	font-size: 1.6rem;
}
h3,
.h3 {
	font-size: 1.45rem;
}
h4,
.h4 {
	font-size: 1.3rem;
}
h5,
.h5 {
	font-size: 1.15rem;
}
h6,
.h6 {
	font-size: 1rem;
}
small {
	font-size: 80%;
}
template {
	display: none;
}
article,
aside,
footer,
header,
nav,
section {
	display: block;
}
hr {
	-webkit-box-sizing: content-box;
	box-sizing: content-box;
	overflow: visible;
	height: 1px;
	border: 0;
	background: #dedede;
	background: currentColor;
	background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.15), rgba(0, 0, 0, 0));
}
b,
strong {
	font-weight: bolder;
}
audio,
canvas,
video {
	display: inline-block;
	*display: inline;
	*zoom: 1;
}
audio:not([controls]) {
	display: none;
}
progress {
	display: inline-block;
	vertical-align: baseline;
}
pre {
	overflow: auto;
	-ms-overflow-style: scrollbar;
}
code,
pre,
samp {
	font-family: Consolas, Menlo, Courier, monospace;
	font-size: 1em;
	text-shadow: none;
}
img {
	vertical-align: middle;
	display: inline-table;
	border-style: none;
}
button,
input {
	overflow: visible;
}
button,
select {
	text-transform: none;
}
button,
input,
select,
textarea {
	outline: none;
	font-family: inherit;
	font-size: inherit;
	line-height: inherit;
	color: inherit;
}
textarea {
	overflow: auto;
	vertical-align: top;
}
button,
[type='button'],
[type='reset'],
[type='submit'] {
	-webkit-appearance: button;
}
button::-moz-focus-inner,
[type='button']::-moz-focus-inner,
[type='reset']::-moz-focus-inner,
[type='submit']::-moz-focus-inner {
	border-style: none;
	padding: 0;
}
sub,
sup {
	font-size: 75%;
	line-height: 0;
	position: relative;
	vertical-align: baseline;
}
sub {
	bottom: -0.25em;
}
sup {
	top: -0.5em;
}
input::-ms-clear,
input::-ms-reveal {
	display: none;
}
a {
	color: inherit;
	text-decoration: none;
}
a:hover {
	opacity: 0.9;
}
mark {
	background-color: #ff0;
	color: #000;
	display: inline-block;
}
em {
	color: #fe6135;
	font-style: normal;
}

::-webkit-scrollbar {
	width: 6px;
	height: 6px;
}

::-webkit-scrollbar-corner {
	background-color: transparent;
}

::-webkit-scrollbar-thumb {
	background-color: #dedee4;
	border-radius: 4px;
}

::-webkit-scrollbar-track {
	background-color: transparent;
}

```