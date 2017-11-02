##  HTML 语义化
1. 合理使用标签和代码结构，使代码更具可读性，同时让浏览器的爬虫和机器更好的解析
2. 要实现 HTML 语义化需要开发者掌握常用 HTML 标签，理解各种标签所代表的含义，在不同场景使用不同的标签，尽量不使用无意义的标签


##  内容与样式分离原则
1. HTML 写结构，CSS 写样式，JS 写动画及交互事件，三种尽量相互分离，不把样式或者事件写到 HTML 里，便于后期维护
2. 在修改样式的时候，不要使用 JS 直接修改样式，而是通过操作 class 来控制样式变化

##  常见 `<meat>` 标签
> The <meta> tag provides metadata about the HTML document. Metadata will not be displayed on the page, but will be machine parsable.

> `<meta>`元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。

`<meta>` 标签位于文档的头部，不包含任何内容。<meta> 标签的属性定义了与文档相关联的名称/值对。

1. 告诉浏览器解码方式：
```
<meta charset="UTF-8">
```

2. IE=edge告诉浏览器若是IE内核使用IE=edge最新的引擎渲染网页，chrome=1则可以激活Chrome Frame，采用谷歌浏览器内核去渲染页面：
```
<meta http-equiv="X-UA-Compatible" content="ie=edge,chrome=1">
```

3. 告诉爬虫、搜索引擎关键字，你快来找我：
```
<meta name="keywords" content="前端学习">
<meta name="description" content="前端学习网站">
//搜索引擎：好的好的，我知道你是谁了。
```

4. 告诉手机移动端页面正常的显示页面，别缩放啦，啥年代啦:
```
  <meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1">
```

##  文档声明的作用?严格模式和混杂模式指什么?<!doctype html> 的作用?

DOCTYPE是用来声明文档类型和DTD（document type definition，文档类型定义）规范的，一个主要的用途便是文件的合法性验证。 如果文件代码不合法，那么浏览器解析时便会出一些错误。。 。

文档声明的作用就在于告诉浏览器你是用什么类型的文档，然后浏览器就能按照你的文档类型进行解析，不然浏览器就会用混杂模式解析文档，不同的浏览器显示的效果都不太一样。

**严格模式和混杂模式**

严格模式即标准模式，是指浏览器按照 W3C 标准解析代码。

混杂模式又称怪异模式或兼容模式，是指浏览器用自己的方式解析代码。

但是HTML5 没有 DTD ，因此也就没有严格模式与混杂模式的区别，HTML5 有相对宽松的语法，实现时，已经尽可能大的实现了向后兼容。（ HTML5 没有严格和混杂之分）

##  浏览器乱码的原因是什么？如何解决

浏览器乱码的原因：
1. 保存的编码格式和浏览器解析的解码格式不匹配
2. 乱码一般是英文以外的字符

解决：保存文件时确定以哪种编码方式保存，告诉浏览器用这种方式解码。比如文件是 UFT-8 格式的，那么在`<head>`里添加`<meta charset="utf-8">`。

## 常见的浏览器内核，浏览器内核的理解
- Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
- Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等
- Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
- Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]

浏览器内核主要分为渲染引擎和 JS 引擎。
- 渲染引擎： 负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。
- JS引擎则：解析和执行javascript来实现网页的动态效果。
最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。

## HTML5 新增哪些内容或 API
- 文件声明类型：`<!DOCTYPE html>`
- 新元素： `section,nav,aside,video,audio,progress,canvas,figcaption, figure, footer, header...`
- input 元素新类型：url,email,date

document.querySelector():根据css选择器返回第一个匹配的元素，如果没有匹配返回null

document.querySelectorAll():返回的是元素数组，querySelector返回的是一个元素。如果querySelectorAll没有匹配的内容返回的是一个空数组。

classList 操作元素 class ，有 add 和 remove 两个方法。

## input 和 textarea 的区别
- input 是单行文本框，`type="text"`，有value属性。
- textarea是标签对，使用cols、rows 定义文本框大小内容写在标签对内，没有 value 属性，是多行文本框。
```
 <textarea name="" id="" cols="30" rows="10"></textarea>
 <input type="text" value="">
```

