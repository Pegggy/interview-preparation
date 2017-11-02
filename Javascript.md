## null 和 undefined 的区别

## 前端安全
### XSS：跨站脚本攻击
将恶意代码植入到提供给其他用户使用的页面中，简单理解为JavaScript代码注入

防御措施：
- 过滤转义输入输出
- 避免使用 `eval / new Function` 等执行字符串的方法，除非确定字符串和用户输入无关
- 使用cookie的httpOnly 属性，加上属性的cookie字段，js无法进行读写
- 使用 innerHTML 、 document.write 的时候，如果数据是用户输入的，需要对关键字符进行过滤和转义

###  CSRF：跨站请求伪造
跨站请求伪造。其原理是攻击者构造网站后台某个功能接口的请求地址，诱导用户去点击或者用特殊方法让该请求地址自动加载。用户在登录状态下这个请求被服务端接收后会被误以为是用户合法的操作。对于 GET 形式的接口地址可轻易被攻击，对于 POST 形式的接口地址也不是百分百安全，攻击者可诱导用户进入带 Form 表单可用POST方式提交参数的页面。

网站中的一些提交行为，被黑客利用，在访问黑客网站的时候进行的操作，会被操作到其他网站上

防御措施：
- 检测http referer是否同域名
- 避免登录session长时间存储在客户端中
- 关键请求使用验证码或者token机制

## 实现继承

### 借用原形链实现继承
```js
function Parent(name){
  this.name = name
}
Parent.prototype.sayHi = function(){
  console.log(`hi~ My name is ${this.name}`)
}

function Children(name){
   Parent.call(this,name)
}
Children.prototype = new Parent()
```
缺点： 原型对象属性共享,把Parent原型上的方法修改以后，children同样发生改变，修改children同理，还需要把Children.prototype.constructor指向自己。

### 组合式继承
```js
function Person(name, sex){
    this.name = name;
    this.sex = sex;
}

Person.prototype.printName = function(){
    console.log(this.name);
};

function Male(name, sex, age){
    Person.call(this, name, sex);
    this.age = age;
}

Male.prototype = Object.create(Person.prototype);
Male.prototype.constructor = Male

// 在继承函数之后写自己的方法，否则会被覆盖
Male.prototype.printAge = function(){
    console.log(this.age);
};
```

## 前端性能优化
网页加载速度更快、代码执行速度、让用户感觉很快

原因： 文件大、网速慢（用户网速慢、服务器网速慢）、请求多

- 合并请求： 合并css、js文件，制作雪碧图，减少http请求次，节省网络请求时间
- 域名拆分： 增加浏览器下载的并行度，让浏览器同时发起更多请求。

将js、css和图片分别使用3个域名加载，拆分为3个域名可以降低加载速度，但是过多的域名会带来DNS解析时间的损耗，可能会降低性能。

- 开启gzip： 将资源在服务器进行压缩，然后发给浏览器再进行压缩，这种方式会降低传输大小，提高网页加载性能。
- 开启keepAlive： 减少浏览器与服务器建立连接的次数，从而节省建立连接的时间。但是开启keepAlive也会使服务器负载变大，也更加容易遭受攻击，因此在实际项目中需要权衡利弊。
- js、css文件压缩，图片压缩，取出空格、换行和注释，对于js还可以进行变量名替换，把长变量名替换成短变量名，可以通过uglifyjs等进行优化。
- 静态资源cdn分发： 客户端可以通过最佳的网络链路加载静态资源

## AJAX
ajax的主要作用就是实现异步加载数据，可以在页面不刷新的情况下雨服务器进行交换数据，并取得返回的数据。通过JavaScript DOM将ajax获得的数据显示在页面上。
ajax的核心对象就是XMLhttpRequest。

### HTTP请求组成
1. 请求动作或方法：GET/POST
2. 请求地址
3. 请求头：客户端环境信息、身份验证
4. 请求体：客户提交的查询字符串信息、表单信息等

### HTTP请求的响应
1. 状态码：显示请求成功还是失败
2. 响应头：响应头也和请求头一样包含许多有用的信息，例如服务器类型，日期时间，内容类型和长度等
3. 响应体：也就是响应正文

### HTTP状态码
1. 1XX，信息类，表示WEB浏览器受到请求，正在进行进一步处理；
2. 2xx，成功，表示用户请求被正确接收，理解和处理，例如 200 OK；
3. 3XX，重定向，表示请求没有成功，客户必须采取进一步的动作；
4. 4XX，客户端错误，表示客户提交的请求有错误，例如 404 NOT FOUND，意味着请求中引用的文档不存在；
5. 5XX，服务器错误，表示服务器不能完成对请求的处理，如 500。

第一步 var request=new XMLHttpRequest(); new一个XHR对象；
第二步 request.open('GET','get.php',true); open一个方法
第三步 request.send(); 发送数据
第四步 request.onreadystatechange=function(){
if(request.readyState === 4 && request.status === 200){
   //做一些事情，request.responseText
   }
} //对响应进行监听：判断响应结果

## 响应式
实现在不同屏幕分辨率的终端上浏览网页采用不同的展示方式。

### 步骤
1. 设置meta标签
```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```
2. 通过媒体查询设置样式
分辨率小与 980px的情况下采用如下渲染方式
```css
@media screen and (max-width: 980px) {
  #head { … }
  #content { … }
  #footer { … }
}
```
3. 注意点  

宽度使用百分比：
```
#head { width: 100% }
#content { width: 50%; }
```

 处理图片缩放的方法:简单的解决方法可以使用百分比，但这样不友好，会放大或者缩小图片。那么可以尝试给图片指定的最大宽度为百分比。假如图片超过了，就缩小。假如图片小了，就原尺寸输出。
 ```
 img { width: auto; max-width: 100%; }
 ```
 
 ## 浏览器兼容
 不同版本浏览器可以使用autoprefixer，IE Hack，normailize.css 
 
 看需要具体兼容哪个浏览器哪个版本，查兼容方法。
 
 1. 从产品角度出发，产品面向的客户以及客户使用不同浏览器的比例，产品需要保证效果还是保障基本功能。如果是年轻群体则考虑手机端和chrome等比较，如果是政府、银行等传统企业，需要考虑安全性、稳定性和基本功能
 2. 从产品成本角度出发，考虑用户使用低版本IE的比例，如果不大还需要去做兼容吗
 3. 需要兼容到什么程度，是需要和在chrome一样的效果还是，采用别的显示效果就行
 4. 根据兼容要求选择框架或库（bootsrap >= IE8 | jquery1.~ >=IE6  jquery 2.~ >=IE9 | vue >= IE9）
 
 ## 模块化
 目的： 
 1. 解决命名冲突
 2. 依赖管理
 3. 提高代码可读性
 4. 代码解耦，提高代码复用性
 
 ### commonJS
 node 端模块化标准。每个文件都是一个模块，其他文件都可以引用，当需要用的时候直接 require 其他模块，当需要给别的文件用的时候就 module.export 出去。 
 
 node 端 require 是同步的，服务端获取文件是本地获取，是同步的。但是浏览器端是异步的，当 require 文件的时候需要从服务器下载下来，下载下来之后才可以执行其他操作。所以传统 commonJS 在浏览器中无法正常加载。
 
 ### AMD 异步模块定义
 异步模块定义，在浏览器端模块化开发规范。AMD 是 requirejs 在推广过程中对模块定义的规范化的产出。
 
 requirejs 主要解决两个问题:
 1. 多个 js 文件可能存在依赖关系，被依赖的文件要早于依赖它的文件加载到浏览器
 2. js 加载的时候浏览器会停止页面渲染，加载文件越多，页面失去响应时间越长
 
 ## CMD 通用模块定义
 按需加载。
 
 一个文件一个模块，推崇就近依赖。
 
 ## AMD 和 CMD 区别
 1. AMD 推崇依赖前置，在定义模块的时候就要声明其依赖的模块。
 2. CMD推崇就近依赖，只有在用到某个模块的时候再去require。
 
 AMD和CMD最大的区别是对依赖模块的执行时机处理不同，注意不是加载的时机或者方式不同。
 
 同样都是异步加载模块，AMD在加载模块完成后就会执行该模块，所有模块都加载执行完后会进入require的回调函数，执行主逻辑，这样的效果就是依赖模块的执行顺序和书写顺序不一定一致，看网络速度，哪个先下载下来，哪个先执行，但是主逻辑一定在所有依赖加载完成后才执行

 CMD加载完某个依赖模块后并不执行，只是下载而已，在所有依赖模块加载完成后进入主逻辑，遇到require语句的时候才执行对应的模块，这样模块的执行顺序和书写顺序是完全一致的

 这也是很多人说AMD用户体验好，因为没有延迟，依赖模块提前执行了，CMD性能好，因为只有用户需要的时候才执行的原因
 
 ## 闭包
 我的理解就是函数内部嵌套一个函数，把函数内部自定义的变量 return 出来，这样子就可以避免全局变量的污染。
 ## 原型链
 实例化对象原型指向构造函数的 prototype 属性。而原型对象(Array.prototype)本身也是一个对象，它也存在一个指向原型的链接，形成一条原型链。当某个实例化对象需要调用某个方法的时候，会先在其内部的方法中查找，然后会原型链一级一级向上查找，一直查找到 `Object.prototype.__proto__ // null`null 为止。
 ```js
 var arr = [1,2,3]
 arr.__prpto__ ===  Array.prototype // true
 ```
 写出一个类 instanceof 运算符的函数。
 ```js
 function io(obj,fn){
  if(obj.__proto__){
    if(obj.__proto__ === fn.prototype){
      return true
    }else{
      io(obj.__proto__,fn)
    }
  }
  return false
 }
 ```
 ## jQuery 插件开发 / 怎么自己实现一个 jquery
 
 ## 拖曳 canvas 元素
 
 ## 跨域
 
 ## HTTP 协议
 
 ## 浏览器缓存
 
 ## 事件模型及事件代理
 事件有三个阶段：捕获、目标和冒泡阶段。低版本 IE 不支持捕获，只能把事件添加在冒泡阶段。
 
 事件代理的优势是：
 1. 可以节省大量内存占用，减少事件注册，比如在 ul 上代理所有 li 的  click 事件。
 2. 当新增子对象时不需要再次对其进行事件绑定，对于动态部分尤为合适。
 
 ### DOM0
 ```js
 var btn = document.getElementById('#myBtn')
 btn.onclick = function(){
  alert(this.id)
 }
 ```
 DOM0级事件处理程序中 this 指向当前元素，在该例中指向 btn 元素，可以通过 this 访问 btn 元素的任何属性和方法。
 
 删除 DOM0级方法指定的事件处理程序，只要把处理程序的值设为 null 即可。
 ```
 btn.onclick = null
 ```
 ### DOM2
 DOM2级事件处理事件的函数接受三个参数：要处理的事件名、事件处理程序的函数和布尔值。如果布尔值为 true，表示在捕获阶段调用事件处理程序，如果为 false，表示在事件冒泡阶段调用事件处理程序。
 
 和DOM0级事件一样，this 也是指向触发元素的作用域中运行。DOM2级事件处理程序可以按添加顺序依次触发，而 DOM0级会被覆盖。

### IE 事件处理程序
attachEvent 和 datachEvent接受两个参数：事件处理程序名称和事件处理程序函数。IE事件处理程序会在全局作用域中运行，因此 this = window。

### 事件对象
在触发DOM上某个事件时，会产生一个 event 对象，这个对象中包含着所有与事件有关的信息。

在事件处理程序内部，对象 this 的值始终指向 currentTarget 的值，而 target 则只包含事件的实际目标，如果直接将事件处理程序指定给了目标元素，则 this、currentTarget 和 target 包含相同的值。
```html
<body>
  <button id="btn">button</button>
</body>
```
```js
document.body.onclick = function(e){
    alert(e.target)
    alert(e.currentTarget)
    alert(this)
}
```
当点击 button 触发点击事件时，this 和 currentTarget 都指向被绑定元素及 body 元素，而 target 则指向 button 元素，因为 target 是这个事件真正的目标。如果是点击 body 元素触发 click 事件，那么 target 也是 body 元素。

在 IE 事件处理程序中，在使用 DOM0级添加事件处理程序时，event 对象作为 window 的一个属性存在。
```js
 var btn = document.getElementById('#myBtn')
 btn.onclick = function(){
  var event = window.event
  alert(event.type) // 'click'
 }
 ```
 如果通过 attachEvent()添加事件处理程序，那么也存在一个 event 对象作为参数被传入事件处理程序函数中。
 ```js
 var btn = document.getElementById('#myBtn')
 btn.attachEvent('onclick',function(event){
  alert(event.type) // 'click'
})
```

## extends
Class 可以通过`extends`关键字实现继承
```
class Parent{
}
class Child extends Parent{

}
```
子类必须在 constructor 方法中调用 super 方法，否则新建实例的时候会报错。这是因为子类没有自己的 this 对象，而是继承自父类的 this 对象，然后对其进行加工，如果不调用 super 方法，子类就得不到 this对象。

### super 关键字
super 作为函数调用时指向父类的构造函数。ES6要求子类的构造函数必须执行一次 super() 函数。
```
class Parent{
}
class Child extends Parent{
  constructior(){
    super()
  }
}
```
super()虽然代表了父类的 A 构造函数，但是返回的是子类 B 的实例，即 super 内部的 this 指向 B，因此 super() 相当于`Parent.prototype.constructor.call(this)`。

super 作为对象时，在普通方法中，指向父类的原型对象，在静态方法中，指向父类。
```
class A {
  p(){
    return 2
  }
}
class B extends A{
  constructor(){
    super();
    console.log(super.p()) // 2
  }
}
let b = new B(); 
```
子类 B 中的 super.p() 就是讲 super 当做一个对象使用。这时，super 在普通方法之中，指向 A.prototype，所以 super.p() 指向的就是 A.prototype.p()，因此返回2。

因为 super 对象指向父类的原型对象，所以定义在父类实例上的方法或属性是无法通过 super 调用的。
```
class A {
  constructor(){
    this.p = 2
  }
}
class B extends A{
  constructor(){
    super()
  }
  get p(){
    return super.p
  }
}
let b = new B()
b.p // undefined
```
因为 p 是 A 实例属性，因此 super.p 取不到，如果 p 属性定义在原型上就可以取到。
```
class A {
}
A.prototype.p = 2
class B extends A{
  constructor(){
    super()
    console.log(super.p) // 2
  }
}
let b = new B()
```

