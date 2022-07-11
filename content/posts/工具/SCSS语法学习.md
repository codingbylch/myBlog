---
title: "SCSS语法学习"
date: 2021-01-01T00:00:00+08:00
draft: false
tags: ['基础', 'css']
categories: ['前端']
---
## 1. 变量声明$
// 变量使用**`$`**进行声明。scss编译输出时，变量会被赋予的值所替代
```css
$variable: #f90;
.nav {
  background-color: $variable; // 使用变量
}
```

## 2. 嵌套css规则
// 可以避免重复写css规则
```css
#content {
  article {
    h1 {color: #333;}
    p {margin-bottom: 1.4em;}
  }
  aside {background-color: #eee;}
}
```
 /* 编译后 */
```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

## 3. 父选择器的标识符&
// 如写:hover这种伪类时，并不希望以后代选择器的方式连接，就可以使用到这个标识符&来引用父类：
```css
article a {
    color: blue;
    &:hover { color: red }
}
 /* 编译后 */
article a { color: blue }
article a:hover { color: red }
```

### 在实际开发中也很常用
```css
.store {
  margin: 0 24rpx 24rpx 24rpx;
  width: 702rpx;
  height: 340rpx;
  background: rgba(255, 255, 255, 1);
  border-radius: 12rpx;
  box-shadow: 0rpx -6rpx 24rpx 0rpx rgba(0, 0, 0, 0.08);

  &_header {
    display: flex;

    &_title {
      margin: 24rpx 0 0 24rpx;
      height: 32rpx;
      line-height: 32rpx;
      font-family: PingFangSC-Semibold;
      font-size: 32rpx;
      color: rgba(26, 26, 26, 1);
    }
  }
}
```

## 4.群组选择器的嵌套
```css
.container {
    h1, h2, h3 {margin-bottom: .8em}
}
 /* 编译后 */
.container h1, .container h2, .container h3 { margin-bottom: .8em }
```

## 5.子组合选择器和同层组合选择器：>、+和~
// 可以把它们放在外层选择器后边，或里层选择器前边：
```css
article {
    ~ article { border-top: 1px dashed #ccc }
    > section { background: #eee }
    dl > {
      dt { color: #333 }
      dd { color: #555 }
    }
    nav + & { margin-top: 0 }
}
```

## 6.嵌套属性: 除了CSS选择器，属性也可以进行嵌套。
// 但这种写法也很难保持结构清晰
```css
nav {
    border: {
    style: solid;
    width: 1px;
    color: #ccc;
    }
}
```

## 7.导入SASS文件
// sass的@import规则和css的@import规则不同：css的只有执行到@import时才回去加载，会导致页面加载很慢；<br />// sass的则在生成css时就把相关文件导入


## 8.使用SASS部分文件
// 这里写的不明不白


## 9.默认变量值
// 使用sass的!default标签可以实现这个目的，看例子
```css
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```
// 如果用户在导入你的sass局部文件之前声明了一个$fancybox-width变量，那么你的局部文件中对$fancybox-width赋值400px的操作就无效。<br />// 如果用户没有做这样的声明，则$fancybox-width将默认为400px。


## 10.嵌套导入
// 允许@import命令写在css规则内，允许只在某一个选择器的范围内导入sass局部文件。<br />`@import "/common/toast/toast.scss";`

## 11.原生的CSS导入
// 兼容原生css的导入，当是以下几种情况：<br />// - 被导入文件的名字以.css结尾；<br />// - 被导入文件的名字是一个URL地址<br />// - 被导入文件的名字是CSS的url()值。


## 12.混合器@mixin，先定义再使用
// 用于大段样式的重用
```css
@mixin rounded-corners {
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    border-radius: 5px;
}
```
// 然后通过@include来使用这个混合器
```css
notice {
    background-color: green;
    border: 2px solid #00aa00;
    @include rounded-corners;
}
```

### 12-1.何时使用混合器
// 当你能为混合器想出一个好名字时:)


### 12-2.混合器中的CSS规则
// 混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性
```css
@mixin no-bullets {
    list-style: none;
    li {
      list-style-image: none;
      list-style-type: none;
      margin-left: 0px;
    }
}
```

### 12-3.给混合器传参
// 这种方式跟JavaScript的function很像，@mixin定义"函数"，@include使用
```css
@mixin link-colors($normal, $hover, $visited) {
    color: $normal;
    &:hover { color: $hover; }
    &:visited { color: $visited; }
}
// @include使用
  
a{
    @include link-colors(blue, red, green);
}
```
// Sass最终生成的是：
```css
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```
// sass允许通过语法$name: value的形式指定每个参数的值，而且也不必在乎传参顺序
```css
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```

### 12-4. 默认参数值
// 默认值可以是任何有效的css属性值，甚至是其他参数的引用
```css
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

## 13. 选择器继承

