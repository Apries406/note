---
title: Sass
id: c43916ca-b701-4819-8dd4-b0bad415cc47
date: 2024-03-11 09:57:58
auther: 406
cover: /upload/logo.svg
excerpt: Sass 为什么会出现CSS预处理器？ CSS 不是一种编程语言，仅仅只能用来编写网站样式。在web初期，网站的搭建还比较基础，所需要的样式往往也比较简单。但是随着用户需求的增加以及网站技术的升级，CSS一成不变的写法也渐渐不再满足于项目。没有类似JavaScript这样的编程语言所有的变量、常量以
permalink: /archives/34ac326a-6f1d-4af7-9514-cb881b31fd69
categories:
 - xue-xi-xiao-ji
 - qian-duan
tags: 
 - css
 - sass-scss
---

# Sass

## 为什么会出现CSS预处理器？

CSS 不是一种编程语言，仅仅只能用来编写网站样式。在web初期，网站的搭建还比较基础，所需要的样式往往也比较简单。但是随着用户需求的增加以及网站技术的升级，CSS一成不变的写法也渐渐不再满足于项目。没有类似`JavaScript`这样的编程语言所有的变量、常量以及其他的编程语法，CSS的代码难免会显得臃肿以及难以维护，但是又没有CSS的替代品，于是CSS预处理器就作为CSS的扩展，出现在了前端技术中。

## CSS预处理器(Sass/Scss、Less、Stylus)是什么？

CSS预处理器的概念：

​	CSS预处理器用一种专门的编程语言，进行Web页面样式设计，然后再编译成正常的CSS文件，以供项目使用。CSS预处理器为CSS增加一些编程的特性，无需考虑浏览器的兼容性问题。比如说: Sass(Scss)、Less、Stylus、Turbine、Swithch CSS、Css cacheer、DT CSS等，都属于CSS预处理器。

- Sass/Scss

```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
    font: 100% $font-stack;
    color: $primary-color;
}
```

- CSS

```css
body {
    font: 100% Helvetica, sans-serif;
    color: #333;
}
```

## SASS

英文单词(`Syntactically Awesome Stylesheets`)的简写，可翻译为“语法上很棒的样式表”.

Sass是一种动态样式语言，由Ruby开发者设计和开发，Sass语法属于缩排语法，比CSS多出好些功能(如变量、嵌套、运算、混入、继承、指令、颜色处理、函数等)，更容易阅读。

Sass 的工作方式是, 在Sass源文件中书写代码，然后由Sass程序（Sass编译器/转译器）将其转换为最终浏览器能认识的CSS文件。

### Sass与Scss的关系

Sass 的梭排语法，对于习惯CSS前端的web开发者来说很不直观，也不能将CSS代码加入到Sass里面，因此Sass语法进行了改良，Sass 3就变成了Scss(Sassy css)。与原来的语法兼容，只是用`{}`取代了原来的缩进

### 语法功能拓展

#### 选择器嵌套

#### 父选择器 &

#### 属性嵌套

```scss
a {
    color: #333;
    font: {
        size: 14px;
        family: sans-serif;
        weight: bold;
    }
}
```

#### 占位符选择器 %

占位符选择器%foo 必须通过@extend

有时，需要定义一套样式并不是给某个元素用，而是只通过`@extend`指令使用，尤其在制作Sass样式库的时候，希望Sass能忽略用不到的样式。

例如：

```scss
.button%base {
	display: inline-block;
	margin-bottom: 0;
	font-weight: normal;
	text-align: center;
	vertical-align: middle;
	white-space: nowrap;
	vertical-align: middle;
	-ms-touch-action: manipulation;
	touch-action: manipulation;
	cursor: pointer;
	background-image: none;
	border: 1px solid transparent;
	padding: 6px 12px;
	font-size: 14px;
	line-height: 1.42857143;
	border-radius: 4px;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

.btn-default {
	@extend %base;
	color: #333;
	background-color: #fff;
	border-color: #ccc;
}

.button-success {
	@extend %base;
	color: #fff;
	background-color: #5cb85c;
	border-color: #4cae4c;
}

.button-danger {
	@extend %base;
	color: #fff;
	background-color: #d9534f;
	border-color: #d43f3a;
}

```

### 注释

- `CSS` -> `/* */` 会显示在源码中
- `Scss` -> `/**/  //`
  - `//` 不会编译到源码中
  - `/**/`会编译到源码中

### 变量

`$`符号定义变量

```scss
$border-color
// 效果等同于
$border_color
// 会有覆盖效果
```

#### 变量作用域

##### 局部变量

在选择器中定义的变量，只能在选择器范围内使用

```scss
.container {
    $font-size: 14px;
    font-size: $font-size;
}
```

##### 全局变量

1. 在选择器最外面定义的变量

```scss
$font-size: 16px;
.container {
    font-size: $font-size;
}
.footer {
	font-size: $font-size;
}
```

2. 使用`!global`标志定义全局变量

```scss
.container {
    $font-size: 16px !global;
    font-size: $font-size;
}
.footer {
	font-size: $font-size;
}
```

#### 变量值类型

Scss支持6种主要的数据类型

- 数字, 1,2,13,10px
- 字符串，有引号字符串和无引号字符串, “foo”,baz
- 颜色,blue,#04a3f9,rbga(255,0,0,0.5)
- 布尔值
- 空值,null
- 数组(list)，用空格或逗号做分隔符，1.5em 1em 0 2em; Helvetica, Arial, sans-serif
- maps，相当于JavaScript的Object, (key1: value1, key2: value2)

例如:

```scss
$layer-out: 10;
$border-width: 3px;
$font-base-family: 'Open Sans', Helvetica, Arial, sans-serif;
$top-bg-color: rgba(255, 147, 29, 0.6);
$block-base-padding: 6px 10px 6px 10px;
$blank-mode: true;
$var: null; // 值null是其类型的唯一值。表示缺少值，通常有函数返回以指示缺少结果
$color-map: (
	color1: #000,
	color2: #fff,
	color3: #f00,
);
$fonts: (
	serif: 'Helvetica Neue',
	monospace: 'Consolas',
);
```

使用:
```scss
$layer-out: 10;
$border-width: 3px;
$font-base-family: 'Open Sans', Helvetica, Arial, sans-serif;
$top-bg-color: rgba(255, 147, 29, 0.6);
$block-base-padding: 6px 10px 6px 10px;
$blank-mode: true;
$var: null;
$color-map: (
	color1: #000,
	color2: #fff,
	color3: #f00,
);
$fonts: (
	serif: 'Helvetica Neue',
	monospace: 'Consolas',
);

.container {
	$font-size: 16px !global;
	font-size: $font-size;
	@if $blank-mode {
		background-color: #000;
	} @else {
		background-color: #fff;
	}
	content: type-of($var);
	content: length($var);
	color: map-get($color-map, color2);
}

.footer {
	font-size: $font-size;
}

// * 如果列表中包含空值，则输出的Css中忽略该空值
.wrap {
	font: 18px bold map-get($fonts, 'sans');
}
```

#### 默认值

```scss
$color: #333;
// * 如果$color之前没定义就使用如下的默认值
$color: #666 !default;
.container {
    borer-color: $color;
}
```

### 导入 @import

Sass 拓展了`@import`的功能，允许其导入Scss或Sass文件。被导入的文件将合并编译到同一个Css文件中，另外，被导入的文件所包含的变量或者混合指令(mixin)都可以在导入的文件中使用。

例如：

`public.scss`

```scss
$font-base-color: #333;
```

在`index.scss`里面使用:
```scss
@import "public";
$color: #666;
.container {
    border-color: $color;
    color: $font-base-color;
}
```

**注意：**跟普通Css里面@import的区别

如下几种方法，都将视为普通的Css语句，不会导入任何的Sass/Scss文件:

- 拓展名是`.css`
- 文件名义`http://`开头
- 文件名是`url()`
- `@import`包含`media queries`

```scss
@import "public.css";
@import url(public);
@import "http://xxx.com/xxx";
@import 'landscape' screen and (orientation:landscape)
```

### 混合指令 Mixin Directives 详解

混合指令(`Mixin`) 用于定义可重复使用的样式。混合指令可以包含所有的CSS规则，绝大部分Scss规则，甚至通过参数功能引入变量，输出多元化的样式。

#### 定义与使用混合指令 @mixin

```scss
@mixin mixin-name() {
    // * css 声明
}
```

例1: 标准模式


```scss
// * 定义页面一块区域的基本样式
@mixin block {
 	width: 96%;
    margin-left: 2%;
    border-radius: 8px;
    border: 1px #f6f6f6 solid;
}
// * 使用混入
.container {
    .block {
        @include block;
    }
}

@mixin block-padding($top:0, $right:0, $bottom:0, $left:0) {
    // * 引入默认值后，可缺省传参
    padding-top:$top;
    padding-right:$right;
    padding-bottom:$bottom;
    padding-left:$left;
}

.container {
    @include block-padding(0,20px,0,20px); 
}
.container {
    @include block-padding($left:20px,$top:20px, $right:5px, $bottom:7px) 
}

```

例2：可变参数

```scss
/*
* 定义线性渐变
* @param $derection 方向
* @param $gradients 颜色过渡的值列表
*/

@mixin linear-gradient($direction, $gradients...) {
    background-color: nth($gradients, 1); // * nth(Array, index) 获取数组对应下标元素
    background-image: linear-gradient($direction, $gradients);
}

.table-data {
    @include linear-gradient(to right, #F00, orange, yellow);
}
```

### 继承指令 @extend

```scss
%alert {
	padding: 15px;
	margin-bottom: 20px;
	border: 1px solid transparent;
	border-radius: 4px;
	font-size: 12px;
}

.important {
	font-weight: bold;
	font-size: 14px;
}

.alert-info {
	@extend %alert;
	color: #31708f;
	background-color: #d9edf7;
	border-color: #bce8f1;
}

.alert-success {
	@extend %alert;
	color: #3c763d;
	background-color: #dff0d8;
	border-color: #d6e9c6;
}

.alert-warning {
	@extend %alert;
	color: #8a6d3b;
	background-color: #fcf8e3;
	border-color: #faebcc;
}

.alert-danger {
	@extend %alert;
	@extend .important;
	color: #a94442;
	background-color: #f2dede;
	border-color: #ebccd1;
}
```

### 运算符的基本使用

#### 等号操作符

所有的数据类型都支持等号运算符：

| 符号 | 说明   |
| ---- | ------ |
| ==   | 等于   |
| !=   | 不等于 |

- 数字比较

```scss
$theme: 1;
.container {
    @if $theme == 1 {
        background-color: red
    } @else {
        background-color: blue;
    }
}
```

- 字符串比较

```scss
$theme: "red";
.container {
    @if $theme != "blue" {
        background-color: red
    } @else {
        background-color: blue;
    }
}
```

所有的数据类型均支持相等运算 == 或者 != ，此外，每种数据类型也有其各自支持的运算方式

### 关系运算符

| 符号     | 说明     |
| -------- | -------- |
| < (lt)   | 小于     |
| > (gt)   | 大于     |
| <= (lte) | 小于等于 |
| >= (gte) | 大于等于 |

例如：
```scss
$theme: 3;
.container {
    @if $theme >=5 {
        background-color: red;
    } @else {
        background-color: blue;
	}
}
```

### 逻辑运算符

| 符号 | 说明   |
| ---- | ------ |
| and  | 逻辑与 |
| or   | 逻辑或 |
| not  | 逻辑非 |

例如：

```scss
$width: 100;
$height: 200;
$last: false;
div {
    @if $width > 50 and $height < 300 {
        font-size: 16px;
    } @else {
        font-size: 14px;
    }
    
    @if not $last {
        border-color: red;
    } @else {
        border-color: blue;
    }
}
```

### 数字操作符

| 符号 | 说明 |
| ---- | ---- |
| +    | 加   |
| -    | 减   |
| *    | 乘   |
| /    | 除   |
| %    | 取模 |

以下三种情况`/`将被视为除法运算符号：

- 值或值的一部分，是变量或者函数的返回值
- 值被圆括号包裹
- 如果值是算术表达式的一部分

例:

```scss
$width: 1000px;
div {
    font: 16px/30px Arial, Helvetcia, sans-serif; // * 不运算
    width: ($width/2); // * 使用变量和括号
    z-index:round(10)/2; // * 使用了函数
    height: (500px/2); // * 使用了括号
    margin-left: 5px + 8px/2px; // * 使用了+表达式
}
```

如果需要使用变量，同时又要确保`/`不做除法运算而是完整的编译到CSS文件中，只需要用`#{}`插值语句将变量包裹

## 常见函数的基本使用

- Color
- String
- Math
- List
- Selector

### 流程控制指令  @if @for @each @while

#### @if 

`@if()`函数允许您根据条件进行分支，并仅返回两种可能的其中一种。

语法方式同`if......else`

#### @for

`@for`指令可以在限制的范围内重复输出格式，每次按要求(变量的值)对输出结果做出变动。这个指令包含两种格式：

- `@for $var from <start> through <end>`
- `@for $var from <start> through <end>` 

区别在于`through`和`to`的含义:

- 当使用`through`时，条件范围包含`<start>`和`<end>`的值
- 而使用`to`时条件范围只包含`<start>`的值不包含`<end>`的值
- 另外，`$var`可以是任何变量，`<start>`和`<end>`必须是整数值

例：

```scss
@for $i from 1 to 4 {
    .p#{$i} {
        width: 10px * $i;
        height: 30px;
        background-color: red；        
    }
}

@for $i from 1 through 3 {
    .p#{$i} {
        width: 10px * $i;
        height: 30px;
        background-color: red；        
    }
}
```

#### @each

`@each`指令的格式是`$var in <list>`，`$var`可以是任何变量名，而`<list>`是一连串的值，也就是值列表

#### @while

`@while`指令重复输出格式直到表达式返回结果为`false`。这样可以实现比`@for`更复杂的循环

```scss
$column: 12;
@while $column>0 {
    .col-sm-#{$column} {
        width: $column / 12 * 100%;
    }
    $column: $column - 1;
}
```
