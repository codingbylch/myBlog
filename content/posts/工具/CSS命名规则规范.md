---
title: "CSS命名规则规范"
date: 2021-01-01T00:00:00+08:00
draft: false
tags: ['css', '基础']
categories: ['前端']
---
### 类-class
类名，我通常用横线就像这样<br />.head-logo { … } /*页面头部logo的类名*/<br />.nav-li { … } /*导航条上list的类名*/<br />.link-panel-h2 { … } /*链接模块的标题*/


### ID
id,我通常用驼峰式命名。在我的理解里驼峰式命名专门用在独一无二的变量上。所以我也经常在javascrpt中采用这个方法来定义变量。<br />#navLastLi { … } /*获取到导航条的最后一个list*/<br />#panelContent { … } /*模块的正文部分*/<br />#sidebarFooter { … } /*侧边栏底部模块*/<br />那么，页面模块很多，我们可以这样整理


### 常用的CSS命名规则
头：header　　内容：content/container　　尾：footer　　导航：nav　　侧栏：sidebar<br />栏目：column　　页面外围控制整体布局宽度：wrapper　　左右中：left right center<br />登录条：loginbar　　标志：logo　　广告：banner　　页面主体：main　　热点：hot<br />新闻：news　　下载：download　　子导航：subnav　　菜单：menu<br />子菜单：submenu　　搜索：search　　友情链接：friendlink　　页脚：footer<br />版权：copyright　　滚动：scroll　　内容：content　　标签页：tab<br />文章列表：list　　提示信息：msg　　小技巧：tips　　栏目标题：title<br />加入：joinus　　指南：guild　　服务：service　　注册：regsiter<br />状态：status　　投票：vote　　合作伙伴：partner


#### (1)页面结构
容器: container　　页头：header　　内容：content/container<br />页面主体：main　　页尾：footer　　导航：nav<br />侧栏：sidebar　　栏目：column　　页面外围控制整体布局宽度：wrapper<br />左右中：left right center


#### (2)导航
导航：nav　　主导航：mainbav　　子导航：subnav<br />顶导航：topnav　　边导航：sidebar　　左导航：leftsidebar<br />右导航：rightsidebar　　菜单：menu　　子菜单：submenu<br />标题: title　　摘要: summary


#### (3)功能
标志：logo　　广告：banner　　登陆：login　　登录条：loginbar<br />注册：regsiter　　搜索：search　　功能区：shop<br />标题：title　　加入：joinus 　状态：status　　按钮：btn<br />滚动：scroll　　标签页：tab　　文章列表：list　　提示信息：msg<br />当前的: current　　小技巧：tips　　图标: icon　　注释：note<br />指南：guild　服务：service　　热点：hot　　新闻：news<br />下载：download　　投票：vote　　合作伙伴：partner<br />友情链接：link　　版权：copyright<br /> <br />**我们在使用脚本进行页面动态变换的时候，推荐的方法就是修改类名或者修改id名来修改显示样式，当然，常用的还是类名class。**


### 修改类名-取名规范

#### (1)颜色:使用颜色的名称或者16进制代码,如
.red { color: red; }<br />.f60 { color: #f60; }<br />.ff8600 { color: #ff8600; }

###  

#### (2)字体大小,直接使用’font+字体大小’作为名称,如
.font12px { font-size: 12px; }<br />.font9pt {font-size: 9pt; }

###  

#### (3)对齐样式,使用对齐目标的英文名称,如
.left { float:left; }<br />.bottom { float:bottom; }

###  

#### (4)标题栏样式,使用’类别+功能’的方式命名,如
.barnews { }<br />.barproduct { }


### 注意事项::
1.一律小写;<br />2.尽量用英文;<br />3.不加中杠和下划线;<br />4.尽量不缩写，除非一看就明白的单词.


### 常用css文件名
主要的 master.css　　模块 module.css　　基本共用 base.css<br />布局，版面 layout.css　　主题 themes.css　　专栏 columns.css<br />文字 font.css　　表单 forms.css　　补丁 mend.css　　打印 print.css


### 关于语义化的一些建议:
**在开始之前，我想推荐两种简单的编写较好的CSS代码的指导方针:**<br />1、为CSS类名定义的时候，尽量使用小写字母，如果有两个以上的单词，在每个单词之间使用”-”符或单词首字母大写(第一个单词除外)。如:”main-content”或”mainContent”。 <br /> <br />2、优化CSS代码，仅创建关键主要的CSS类并重新为子元素使用符合HTML标准的标签(h1, h2, p, ul, li, blockquote,…)，例如,不要使用这种哦你那个方式:

1. <div class=”main”> 
1.     <div class=”main-title”>…</div> 
1.     <div class=”main-paragraph”>…</div> 
1. </div> 

 <br />而要这样写:

1. <div class=“main”> 
1.     <h1>…</h1> 
1.     <p>…</p> 
1. </div> 

 <br /> 

### 三栏布局中使用语义化方式的例子
让我们来通过这个简单的例子讲解一下如何为经典的三栏布局使用语义化方式命名:<br />![](https://cdn.nlark.com/yuque/0/2020/png/714353/1596593668159-7836974e-fd29-4122-a04d-3d80a1ee05e0.png#align=left&display=inline&height=226&margin=%5Bobject%20Object%5D&originHeight=226&originWidth=275&size=0&status=done&style=none&width=275)

1. **Container：**“#container“ 就是将你页面中的所有元素包在一起的部分，这部分你还可以命名为: ”wrapper“, “wrap“, “page“.
1. **Header：**“#header” 是网站页面的头部区域，一般来讲，它包含网站的logo和一些其他元素。这部分你还可以命名为:”_top_“, “_logo_“, “_page-header_” (或 pageHeader). <br />
1. **Navbar：**“_#navbar_“等同于横向的导航栏，是最典型的网页元素。这部分你还可以命名为:_“nav”_, _“navigation”_, _“nav-wrapper”_. <br />
1. **Menu：**“#Menu”区域包含一般的链接和菜单，这部分你还可以命名为: ”_sub-nav_ ”, “_links_“. <br />
1. **Main：**“#Main”是网站的主要区域，如果是博客的话它将包含你的日志。这部分你还可以命名为: ”_content_“, “_main-content_” (or “mainContent”)。 <br />
1. **Sidebar：**“#Sidebar” 部分可以包含网站的次要内容，比如最近更新内容列表、关于网站的介绍或广告元素等…这部分你还可以命名为: ”sub-nav“, “side-panel“, “secondary-content“. <br />
1. **Footer：**“#Footer”包含网站的一些附加信息，这部分你还可以命名为: ”_copyright_“.

### HTML5语义化标签
<main> 存放每个页面独有的内容。每个页面上只能用一次 <main>，且直接位于 <body> 中。最好不要把它嵌套进其它元素。<br /><article> 包围的内容即一篇文章，与页面其它部分无关（比如一篇博文）。<br /><section> 与 <article> 类似，但 <section> 更适用于组织页面使其按功能（比如迷你地图、一组文章标题和摘要）分块。一般的最佳用法是：以 标题 作为开头；也可以把一篇 <article> 分成若干部分并分别置于不同的 <section> 中，也可以把一个区段 <section> 分成若干部分并分别置于不同的 <article> 中，取决于上下文。<br /><aside> 包含一些间接信息（术语条目、作者简介、相关链接，等等）。<br /><header> 是简介形式的内容。如果它是 <body> 的子元素，那么就是网站的全局页眉。如果它是 <article> 或<section> 的子元素，那么它是这些部分特有的页眉（此 <header> 非彼 标题）。<br /><nav> 包含页面主导航功能。其中不应包含二级链接等内容。<br /><footer> 包含了页面的页脚部分。
