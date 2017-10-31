## 前端安全
### XSS：跨站脚本攻击
将恶意代码植入到提供给其他用户使用的页面中，简单理解为JavaScript代码注入

防御措施：
- 过滤转义输入输出
- 避免使用 `eval / new Function` 等执行字符串的方法，除非确定字符串和用户输入无关
- 使用cookie的httpOnly 属性，加上属性的cookie字段，js无法进行读写
- 使用 innerHTML 、 document.write 的时候，如果数据是用户输入的，需要对关键字符进行过滤和转义

###  CSRF：跨站请求伪造
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
 
 








