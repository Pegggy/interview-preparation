## CSS盒模型
盒子模型有两种： W3C 盒子模型和 IE 盒子模型。

盒子模型包括：content、padding、margin 和 border
IE 盒子模型的 content 包括 padding 和 border

## link 和 @import
- `<link>`是一个 HTML 标签，除了可以用于导入 CSS 样式表，还可以加载其他资源。@import只能加载 CSS。
- link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
- 由于@import是CSS2.1提出的所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题。

## CSS 选择器
- 元素选择器： `p,div,h1,h2{color: #000}`
- 类选择器：`.class1,.class2`
- id 选择器： `#id1,#id2`
- 通配选择器： ` * `
- 属性选择器： `a[class="button"]`选择 a 标签中 class = "button" 的元素
- 后代选择器： `div h1` 选中 div 下所有 h1 标签
- 子选择器： `div > h1` 选中 div 下直接子元素 h1
- 相邻选择器： `h1 + p {margin: 10px}` 选中紧跟在 h1 后面的出现的 p 元素，如果 h1 后面有两个 p 元素只对第一个 p 元素生效
- 伪类选择器：`a:link	, a:hover, ul:first-child`
- 伪元素选择器：`p:first-line,h1:first-letter,div:before,div:after`

## CSS 优先级
1. `!important`，在属性后面使用!important会覆盖页面内任何位置定义的元素样式
2. 内联样式
3. id选择性指定的样式
4. class选择器指定的样式
5. 伪类选择器指定的样式
6. 属性选择器指定的样式
7. 标签定义的样式
8. * 定义的样式
9. 浏览器样式

### 复杂场景优先级计算

行内样式  ==> 1，0，0，0
ID 选择器 ==> 0，1，0，0
类，属性选择器和伪类选择器 ==> 0，0，1，0
标签选择器、伪元素 ==> 0，0，0，1 

根据元素的样式所占权重进行计算，值越大，样式优先级越高；后面的样式会覆盖前面的样式。

## 块级元素和行内元素
块级元素
```
h1~h6,p,div,form,table,tr,td,ul,ol,li,dd,dt,dl,hr
```
行内元素
```
span,a,input,button,img,strong,em,label,select,textarea,br
```

### 区别
- 块级元素占满一整行，行内元素只占元素自身宽度；
- 可以对块级元素设置width和height，而对行内元素设置width和height无法生效；
- 对行内元素设置上下margin、上下padding时无法生效，只有设置左右margin、padding才能生效，若父级元素是块级元素，则无法感知到行内元素的上下margin、padding，并不会包裹该行内元素；
- 块级元素内部可以放置块级元素和行内元素，而行内元素内只能放行内元素和文本，无法放置块级元素。比如 a 标签内只能放置行内元素，无法放下 ul 等块级元素，如果需要放入块级元素，需要把 a 的样式设置为`display:block;`。

## CSS 继承
CSS继承是指对某元素指定样式时，该样式不仅会对该元素生效，还会将样式应用到它的后代元素中。

### 可继承属性
- 文本属性：text-align,text-indent,text-decoration,text-transform,word-spacing,letter-spacing,line-height,等
- 字体属性：font-size,font-family,font-weight,font-style
- 元素颜色属性：color(不包括背景)
- 列表属性：list-style,list-style-type,list-style-image

### 不可继承属性
一般，大多数框模型属性（包括外边距、内边距、背景、边框等）都无法继承。

- 比如盒子模型属性：margin,padding,border,display,height,width,
- 定位属性：left,top,right,bottom,float,position,z-index,overflow,
- 背景属性：background,background-img,background-repeat,background-color

## 浮动
浮动元素会脱离正常文档流，但是它会影响布局。

- **对父容器来说**，它察觉不到浮动元素的存在；    
- **对其他浮动元素**，后面的浮动元素会受到前面浮动元素的影响，若float方向相同，父元素无法同时容纳下多个子浮动元素，那么的后面浮动元素会向下移动,直到有足够的空间,如果浮动元素的高度不同,那么向下移动的时候可能被卡住。  
- **对普通元素**，当做该浮动元素不存在，按正常文档流显示，占据浮动元素的位置，若用重叠会被浮动元素覆盖住；  
- **对文字等行内元素**，会避开浮动元素显示，形成环绕效果。

### 清除浮动
由于浮动元素脱离正常文档流，父元素无法感知到浮动元素的存在，父元素无法自适应高度。比如当父元素未设置高度，但是子元素全部为浮动元素，会导致父元素高度塌陷，此时需要清除浮动。

1. BFC 清除浮动
- float为 left|right（缺点：父元素长度根据内容自适应）；
- overflow为 hidden|auto|scroll（缺点：overflow属性会影响滚动条和绝对定位的元素）；
- display为 table-cell|table-caption|inline-block（缺点：无法解决低版本IE问题）；
- position为 absolute|fixed（缺点：改变父元素定位方式）。

2. 设置伪元素清除浮动  
```
.clearfix:afert{
    content: "";
    display: block;
    clear: both;
}
.clearfix{
    *zoom:1;
}
```

## 居中
### 水平居中
块级元素水平居中：`{margin: 0 auto;}`

行内元素水平居中：`{text-align: center};`
### 垂直居中
1. table
```
  <table class="parent">
    <tr>
      <td class="child">
      一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
      </td>
    </tr>
  </table>
  
.parent{
  border: 1px solid red;
  height: 600px;
}
.child{
  border: 1px solid green;
}
```

2. 绝对定位：

已知元素宽高
```
// 1
.child{
  width: 300px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -150px;
  height: 100px;
  margin-top: -50px;
}
// 2 
.child{
  position: absolute;
  width: 300px;
  height: 200px;
  margin: auto;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
```
未知元素宽高
```
.child{
  border: 1px solid green;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
```

3. flex
```
.parent{
  height: 600px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.child{
  width: 300px;
}
```
## CSS 隐藏元素的实现
- `opacity: 0` 透明度为0
- `visibility: hidden;` 
- `display: none;`
-  `width: 0; height: 0; overflow: hidden;`
- `position: absolute; left: 1000000px; top: 100000px; `移出可视范围
- `transform: translate(-9999px);` 同上
- `transform: scale(0);`
- `z-index: -1000; ...etc`
- `background-color: #fff;` 把背景颜色设置成 body 一样的颜色
- `font-size: 0;` 隐藏文字







