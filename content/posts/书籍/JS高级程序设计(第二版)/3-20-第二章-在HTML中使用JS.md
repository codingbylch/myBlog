---
title: 第二章 - 在HTML中使用JS
date: 2020-03-20T18:43:28+08:00
tags: [JS高级程序设计, 基础]
categories: [前端, 书籍]

---
本章内容 
- 使用\script元素 
- 嵌入脚本与外部脚本 
- 文档模式对 JavaScript 的影响 
- 考虑禁用 JavaScript 的场景

## 小结
把 JavaScript 插入到 HTML 页面中要使用script元素。使用这个元素可以把 JavaScript 嵌入到HTML 页面中，让脚本与标记混合在一起；也可以包含外部的 JavaScript 文件。而我们需要注意的地方有：
- 在包含外部 JavaScript 文件时，必须将 src 属性设置为指向相应文件的 URL。而这个文件既可以是与包含它的页面位于同一个服务器上的文件，也可以是其他任何域中的文件。 
- 所有script元素都会按照它们在页面中出现的先后顺序依次被解析。在不使用 defer 和async 属性的情况下，只有在解析完前面script元素中的代码之后，才会开始解析后面script元素中的代码。 
- 由于浏览器会先解析完不使用 defer 属性的script元素中的代码，然后再解析后面的内容，所以一般应该把script元素放在页面最后，即主要内容后面，&lt;/body&gt;标签前面。 
- 使用 defer属性可以让脚本在文档完全呈现之后再执行。延迟脚本总是按照指定它们的顺序执行。
- 使用 async 属性可以表示当前脚本不必等待其他脚本，也不必阻塞文档呈现。不能保证异步脚本按照它们在页面中出现的顺序执行。 
另外，使用&lt;noscript&gt;元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启用了脚本的情况下，浏览器不会显示&lt;noscript&gt;元素中的任何内容。 