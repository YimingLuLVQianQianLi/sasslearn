<h3 align="center"> Sass 入门学习</h3>
#### 1.sass是什么？
sass是CSS的扩展语言。
> - Sass是由Hampton Catlin 设想的，然而他不知道编码方式
> - Natalie Weizenbaum是sass主要开发人员
> - Chris Eppstein sass的核心贡献者，是compass框架的作者
#### 2.sass两种语法？
sass扩展名分为两种 .sass和.scss
- .sass语法是缩进语法，这种语法比较旧
- .scss语法是css的语法扩展，这种语法可以方便的把你的css文件改成.scss使用
#### 3.使用sass
- 第一种
    - 安装ruby
    - 安装sass  >  gem install sass
    - 使用命令行运行sass编译 > sass table.scss table.css
    - 监听sass文件，实时编译 > sass --watch table.scss:table.css
    - 监听sass文件夹，编译 > sass public/sass:public/css
- 第二种
    - 通过gulp,grunt,webpack进行编译
#### 4.编译后代码风格
- 命令行使用 sass table.scss table.css --style 
    - nested > css结构每个css选择器以缩进方式展示
    - expanded > 经典显示，每一个选择器和属性独占一行
    - compact >  简单，每个选择器一行显示
    - compressed > 压缩css
#### 5.语法规则 
###### 注：使用expanded风格演示
---
##### 1、嵌套规则  
###### test.scss
> @mixin和@import 是不允许被嵌套的
```
 #wrap div{
     border: 1px solid green;
     height: 300px;
     a {
         color: #000;
         text-decoration: none;
     }
 }
```
###### test.css
```
#wrap div {
  border: 1px solid green;
  height: 300px;
}
#wrap div a {
  color: #000;
  text-decoration: none;
}
```
---
##### 引用父选择器 &
> 说明：应用父级元素，这里的&代表的是a元素。在使用嵌套规则时要使用父级元素时将& 符号插入使用位置即可

###### test.scss
```
#wrap div{
     a {
         color: #000;
         text-decoration: none;
		&:hover{
			color: #ccc;
			text-decoration: underline;
		}
     }
}
```
###### test.css
```
#wrap div a {
  color: #000;
  text-decoration: none;
}
#wrap div a:hover {
  color: #ccc;
  text-decoration: underline;
}
```
---
##### 变量 $
> 说明：使用$开头定义变量,
>> 如：$width: 200px; 
###### test.scss

```
//定义变量
$height: 200px;
#wrap div{
    //使用变量
    height: $height;
}
```
###### test.css
```
#wrap div {
  height: 200px;
}

```
---
##### 计算功能
> 说明：
> - 命令行sass -i 可以打开控制台里边进行演示计算
> - 可以进行+ - * / 
> - 如：1px +1px ,#000+#111 , $w -$h ,10px/2, $w* 10等
###### test.scss
```
$height: 200px;
#wrap div{
    height: $height + 200px;
    background: #ccc + #111;
}
```
###### test.css
```
#wrap div {
  height: 400px;
  background: #dddddd;
}

```
---
##### @import
> 说明：
> - 插入外部的文件 如：插入test.scss 就可以使用 @import 'test'
> - 插入内部的sass文件在编译后相当于直接复制到文件当前里边在编译
> - 区别于css的@import,css的@import在使用的时候会想服务器请求这个文件，而sass是不会的
> - 如果想插入css文件可以 @import 'test.css' ，则sass直接会识别这是插入的css文件
> - 
###### test.scss
```
//有一下几个规则编译的时候会当成css的improt如：
//1.扩展名为.css
//2.文件名以http://
//3.文件名 url()
//4.@import有媒体查询的
@import "test.css";
@import "test" screen;
@import "http://soso.com/test";
@import url(css/table.css)
//以下会当sass文件插入 ，这里不做演示，如果文件存在会直接插入被编译后css文件中
@improt "test.scss";
@import "test";
@import "test1", "test2"

```
###### test.css
```
@import url(test.css);
@import "test" screen;
@import "http://soso.com/test";
@import url(css/table.css);
```
---
##### 继承 @extend
> 说明：  
> - 当一个选择器要继承一个另一选择器所有样式的时候
> - 可以在一个选择器继承多个选择器
> - 可以在连续扩展，一个扩展另一个，另一个在扩展下一个
###### test.scss
```
.common {
	margin: 0;
	padding: 0;
}
ul , li ,dl , dd , p {
	@extend .common;
	font-size: 14px ;
	color: green;
}
#wrap {
	@extend .common;
	background: #ccc;
}
    
```
###### test.css
```
.common, ul, li, dl, dd, p, #wrap {
  margin: 0;
  padding: 0;
}

ul, li, dl, dd, p {
  font-size: 14px;
  color: green;
}

#wrap {
  background: #ccc;
}

```
---
##### 混合 @mixin
> 说明：
> - 它跟宏很像，也可以说成是代码片段 
> - 有两种使用方式， 带参数和不带参数
> - 定义一个@mixin name >  使用@include name  
> - @mixin和@import 是不允许被嵌套的
###### mixin.scss
```
@charset "UTF-8";
/*定一个普通的@mixin*/
@mixin clearfix {
	&:after {
		content: ' ';
		display: block;
		font-size: 0;
		clear: both;
	} 
}
#wrap {
	@include clearfix;
}
/* 定义一个包含@mixin的@mixin*/
@mixin clearfix-boxder {
	border: 1px solid #ccc;
	@include clearfix;
}

#wrap {
	@include clearfix-boxder;
}
/* 带参数*/
@mixin param( $width, $height, $color) {
	width: $width;
	height: $height;
	background-color: $color
} 

#wrap {
	@include param (200px, 300px, #ccc)
}
/* 变量参数...引用参数列表*/
$values: 100px, 100px, green;
#wrap {
	@include param($values...);
} 
```
###### mixin.css
```
@charset "UTF-8";
/*定一个普通的@mixin*/
#wrap:after {
  content: ' ';
  display: block;
  font-size: 0;
  clear: both;
}

/* 定义一个包含@mixin的@mixin*/
#wrap {
  border: 1px solid #ccc;
}
#wrap:after {
  content: ' ';
  display: block;
  font-size: 0;
  clear: both;
}

/* 带参数*/
#wrap {
  width: 200px;
  height: 300px;
  background-color: #ccc;
}

/* 变量参数...引用参数列表*/
#wrap {
  width: 100px;
  height: 100px;
  background-color: green;
}

```
---
##### 注释
> 说明：
> -  /* */ 编译会在编译后的css中显示，但是压缩模式会删除这些注释
> -  /*! */ 重要注释压缩也会保留
> -  // 单行注释，只显示在sass文件
> - 
###### comments.scss
```
@charset "UTF-8";
//单行注释，只在当前文件显示
a {color: green;}

/*
* 编译后还会存在，压缩会删除
*/
b {
	color: red;
}
/*! 编译、压缩后都会存在 */ 

body {
	margin:0;
}
```

###### comments.css
```
@charset "UTF-8";
a {
  color: green;
}

/*
* 编译后还会存在，压缩会删除
*/
b {
  color: red;
}

/*! 编译、压缩后都会存在 */
body {
  margin: 0;
}

```
##### 插入值 #{}
###### interpolation.scss
```
@charset "UTF-8";
$el: a;
$class: content;
$val: 10px;
$val1: 2px;
#wrap {
	#{$el} {
		color: #c#{$el}c;
	}
	&-#{$class} {
		color: red;
		width: $val/$val1;
		//区别，#{}会识别它是css操作而不做运算
		font: #{$val}/#{$val1};
	}
}
```
###### interpolation.css
```
#wrap a {
  color: #cac;
}
#wrap-content {
  color: red;
  width: 5;
  font: 10px/2px;
}
```

## 高级操作
---
##### 变量的高级操作
> - 提升嵌套内部变量的作用域 !global
###### adv-variables.scss
```
@charset "UTF-8";
#wrap {
	$local_width: 40px;
	border-width: $local_width - 39px;
	a {
		display: inline-block;
		width:$local_width;
	}
}
#wrap1 {
	/* width: $local_width; */
	/* 这里是不能使用的，这样写编译会报错 */
}
/*提升嵌套内部的变量为全局*/ 
#wrap {
	$local_one: 40px !global;
	border-width: $local_one - 39px;
	a {
		display: inline-block;
		width: $local_one;
	}
}
#wrap2 {
	width: $local_one;
}
```
###### adv-variables.css
```
@charset "UTF-8";
#wrap {
  border-width: 1px;
}
#wrap a {
  display: inline-block;
  width: 40px;
}

#wrap1 {
  /* width: $local_width; */
  /* 这里是不能使用的，这样写编译会报错 */
}

/*提升嵌套内部的变量为全局*/
#wrap {
  border-width: 1px;
}
#wrap a {
  display: inline-block;
  width: 40px;
}

#wrap2 {
  width: 40px;
}

```
> -  数据类型

类型 | 例如
---|---
数字 | 8.8,14,66px
字符串（引号和没有引号） | "string", 'strging' ,string
颜色| green,#fff, rgba(255,255,15,0.2)
布尔值| true ,false
空|null
空格或逗号分隔开的列表| 1em 3em 3em , 宋体,arial,黑体
键值对| (key1:value1,key2:value2,key3:value3) 
|注：支持!important声明
> 
> 1. 带引号字符串的调用方式
> 2. 数字操作 + - *  / % （模除） sass计算可以保留单位（单位必须相同） 关系运算 > < >= >= == !=
###### adv-string.scss
```
@charset "UTF-8";
$content: "content";
#wrap {
	&:after{
		content: $content; //带引号引用
		#{$content}: "你好"; //不带引号引用
		width: 10px !important;
	}
	//运算 
	#content {
		font: 8px/10px;
		margin-left: 8px/2;
		padding-left: 1px + 8px/2px;
		padding-right: round(2.2) +2px;
		$v:28px;
		$v1:20px;
		left: $v/$v1;
		font: #{$v}/#{$v1};
		right: $v/2;
		//减号和负号
		bottom: -23px;
		margin: 1px -24; //负号
		margin: 1px -$v1;  //减号
		margin: 12px-2px; //减号
		background-position: -$v1;//负号

	}
	//色彩
	#color{
		color: #000+#111;
		color: #111 * 2;
		opacity: 0.2;
	}

	$def: #ccc !default;
	//$def: blue; //默认值的使用
	#bb{
		background: $def;
	}
}
```
###### adv-string.css
```
@charset "UTF-8";
#wrap:after {
  content: "content";
  content: "你好";
  width: 10px !important;
}
#wrap #content {
  font: 8px/10px;
  margin-left: 8px/2;
  padding-left: 5px;
  padding-right: 4px;
  left: 1.4;
  font: 28px/20px;
  right: 14px;
  bottom: -23px;
  margin: 1px -24;
  margin: -19px;
  margin: 10px;
  background-position: -20px;
}
#wrap #color {
  color: #111111;
  color: #222222;
  opacity: 0.2;
}
#wrap #bb {
  background: #ccc;
}

```
---
##### @extend高级应用
> - % . # 占位符 会替换这些开头的选择器
> -  !optional如： @extend name !optional, 当name不存在的时候不会报错
###### adv-extend.scss
```
@charset "UTF-8";
// 占位符 在这里. # % 会当做占位符来使用

#wrap div%b_color {
	color: #ccc;
}

.one {
	@extend %b_color;
	margin:0;
	padding:0;
}
#two {
	@extend %content !optional; //没有这个也不会报错
	font-size: 12px;
}

#three {
	@extend .one //扩展继承
}
```
###### adv-extend.css
```
#wrap div.one, #wrap div#three {
  color: #ccc;
}

.one, #three {
  margin: 0;
  padding: 0;
}

#two {
  font-size: 12px;
}
```
##### @at-root 
> 是嵌套里边标签成为根标签
##### @debug
> 在控制台里边打印，用于调试
##### @error
> 错误信息
##### @warn 
> 告警
```
@charset "UTF-8";
#wrap {
	font-size: 14px;

	@at-root {
		//不被包含在里边编译
		#footer {
			font-size: 12px;
		}
		#header {
			font-size: 12px;
		}	
	}
	@debug 10px + 10px; // 打印出来
	#cotent {
		font-size: 12px;
		@warn "content font-size: 12px"
	}
}

@mixin error($a, $b) {
	@warn "a的值是 #{$a}";//告警用于调试
	@if 12px != $b {
		 @error "$b不等于12px"; //报错误使用	
	} 
	width: $a;
}
#wrap1 {
	@include error(12px, "12px");
	//@include error(12px, 14px); //有错误回编译失败
}
```

##### if(), @if, @for, @each, @while 
> 说明： 控制指令和表达式 大部分情况运用到@mixin中
> - if(条件，值1，值2) 为条件真时为值1，假时值2，可以看做是三元表达式
> - @if 条件 {执行代码}  
> - @if 条件 {执行代码} @else if 条件 {执行代码} @else {执行代码} 
> - @for 变量名 from 开始 through 结束 ****包含开始结束值
> - @for 变量名 from 开始 to 结束     ****不包含开始结束值
> - @each 变量名 in 列表或者map {执行代码块}
> - @while 条件 { }  ****直到条件为假时停止
##### control.scss
```
@charset "UTF-8";
@mixin if1($isT) {
	font-size: if($isT, 12px, 14px);  //if例子
}
@mixin if2($isBackground,$color) {
	@if $isBackground {background: $color;}	 //@if例子
	//  @ if 2 > 1 {display: block;}
	//  @ if null {display: none;}
}
@mixin if3($type) {
	// 多重判断
	@if $type == white {
		background: #fff;
	} @else if $type == black {
		background: #000;
	} @else {
		background: green;
	}
}
@mixin dt_w {
	@for $w from 10 through 16 { //for 用法
		.dt_w#{$w*10} dt{
			min-width: $w * 10px;
		}
	}
}
@mixin contextMenu($list) { //each实战用法
	#contextMenu dl dt span.public_img{
		 background-image: url(../img/contextMenu/public_contextMenu.png);
	}
	@each $name in $list {
		#contextMenu dl dt span.#{$name} {
			@extend .public_img;
		}	
	}
	
}
@mixin contextMenuPosition($map){ //each map实战用法
	@each $key,$value in $map {
		#contextMenu dl dt span.#{$key} {
			background-position: $value
		}
	}
}
@mixin while { //while用法
	$val: 160;	
	@while $val > 100 {
		.dt_w#{$val} {
			min-width: $val + px;
		}
		$val: $val - 10;	
	}
}
#wrap {
	@include if1(true);
}

#wrap {
	@include if2(true, green);
}
#wrap {
	@include if3(white);
}
@include dt_w;

$names: create,edit,del;
@include contextMenu($names);

$namesMap: (create:0 2px, edit: 0 10px, del: 0 18px);
@include contextMenuPosition($namesMap);

@include while;
```
##### control.css
```
#wrap { font-size: 12px; }

#wrap { background: green; }

#wrap { background: #fff; }

.dt_w100 dt { min-width: 100px; }

.dt_w110 dt { min-width: 110px; }

.dt_w120 dt { min-width: 120px; }

.dt_w130 dt { min-width: 130px; }

.dt_w140 dt { min-width: 140px; }

.dt_w150 dt { min-width: 150px; }

.dt_w160 dt { min-width: 160px; }

#contextMenu dl dt span.public_img, #contextMenu dl dt span.create, #contextMenu dl dt span.edit, #contextMenu dl dt span.del { background-image: url(../img/contextMenu/public_contextMenu.png); }

#contextMenu dl dt span.create { background-position: 0 2px; }

#contextMenu dl dt span.edit { background-position: 0 10px; }

#contextMenu dl dt span.del { background-position: 0 18px; }

.dt_w160 { min-width: 160px; }

.dt_w150 { min-width: 150px; }

.dt_w140 { min-width: 140px; }

.dt_w130 { min-width: 130px; }

.dt_w120 { min-width: 120px; }

.dt_w110 { min-width: 110px; }

```
---
参考文献 
- 官方文档 http://sass-lang.com/



