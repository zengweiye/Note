# KnowLedge

标签（空格分隔）： HTML CSS JavaScript Vue

---
## 1.同源
- 主机、协议、端口相同即为同源，限制脚本或文档与不同源资源的交互


## 2.数据格式
- enctype：application/x-www-form-urlencoded：数据格式为键值对，表单提交默认的标准编码格式
- enctype：multipart/form-data：表单提交文件时的数据格式
- content-type：application/json：ajax提交的默认形式，以json数据的形式传给后端


## 3.正则表达式验证
- 验证只能含有6-16位大小写字母、数字、下划线：
^[\w]{6,16}\$
- 验证字符串是否全是空格组成：
^[\s]+\$
- 验证只能含有中文、大小写字母、数字、下划线（不含中文符号）：
^[\u4E00-\u9FA5\w]+\$
- 验证邮箱格式：
^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]+\$
- 验证只能含有大小写字母和数字
^[a-zA-Z0-9]+\$
- 验证是否为邮箱或手机号
^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]+\$|\d{11}


## 4.浏览器兼容问题
- 浏览器内外补丁区别：*{margin:0;padding:0}
- 使用条件注释，在不同浏览器情况下使用不同的js包
- 对于内置对象的调用可先判断其是否存在再进行调用，否则使用对应的浏览器的调用方法
- CSS设计上不同属性


## 5.浏览器内核
- 使用Trident内核的浏览器：IE、Maxthon、TT； -ms-
- 使用Gecko内核的浏览器：Netcape6及以上版本、FireFox； -moz-
- 使用Presto内核的浏览器：Opera7及以上版本； -o-
- 使用Webkit内核的浏览器：Safari、Chrome。 -webkit-


## 6.reflow（回流）与repaint（重绘）
- reflow：子元素改变引发兄弟元素以及父元素的改变，需要进行reflow
- repaint：改变元素的背景色、文字颜色、边框颜色、outline等不影响周围与内部布局的属性
- 减少回流
  - 尽量只改变子元素
  - 尽量使用class设计元素格式
  - 对于经常需要回流的组件，position设为fixed或absolute
  - 权衡速度的平滑，动画改变从1像素单位改为3像素单位
  - 对于元素结点的修改可以先从dom结点中抽离操作完毕再display到页面上
  - 减少不必要的dom层级以及复杂的css选择器


## 7.浏览器渲染原理
 1. 浏览器将获取的HTML文档并解析成DOM树。display:none也会出现在DOM树，CSS加载不会阻塞DOM树的解析。JavaScript加载会阻塞DOM树的解析，可通过设置defer和async属性进行异步加载延迟执行
 2. 处理CSS标记，构成层叠样式表模型CSSOM(CSS Object Model)CSS执行会与JavaScript互斥， 可以DOM树解析同时进行
 3. 将DOM和CSSOM合并为渲染树(rendering tree)将会被创建，代表一系列将被渲染的对象。 display:none不在树内;visibility:hidden在树内，CSS的加载会阻塞DOM的渲染。
 4. 渲染树的每个元素包含的内容都是计算过的，它被称之为布局layout。浏览器使用一种流式处理的方法，只需要一次pass绘制操作就可以布局所有的元素。float、absolute、fixed会发生位置偏移，脱离文档流即脱离渲染树
 5. 将渲染树的各个节点绘制到屏幕上，这一步被称为绘制painting.


## 8.GET和POST
1. 发送的数据数量
在 GET 中，只能发送有限数量的数据，因为数据是在 URL 中发送的。
在 POST 中，可以发送大量的数据，因为数据是在正文主体中发送的。
2. 安全性
GET 方法发送的数据不受保护，因为数据在 URL 栏中公开，这增加了漏洞和黑客攻击的风险。
POST 方法发送的数据是安全的，因为数据未在 URL 栏中公开，还可以在其中使用多种编码技术，这使其具有弹性。
3. 加入书签中
GET 查询的结果可以加入书签中，因为它以 URL 的形式存在；而 POST 查询的结果无法加入书签中。
4. 编码
在表单中使用 GET 方法时，数据类型中只接受 ASCII 字符。
在表单提交时，POST 方法不绑定表单数据类型，并允许二进制和 ASCII 字符。
5. 可变大小
GET 方法中的可变大小约为 2000 个字符。
POST 方法最多允许 8 Mb 的可变大小。
6. 缓存
GET 方法的数据是可缓存的，而 POST 方法的数据是无法缓存的。
7. 主要作用
GET 方法主要用于获取信息。而 POST 方法主要用于更新数据。



## 10.状态码
- 100 服务请求中
- 101 切换协议，服务端根据客户端要求切换到更高的协议
- 200 服务请求成功
- 201 created 已创建，成功请求并创建了新的资源
- 202 accepted 已接受，已接受但并未处理完成
- 204 No Content无内容，服务器成功处理但未返回内容
- 301 Moved Permanently 永久移动，返回信息包括新URI，浏览器以后会自动重定向到新URI
- 302 Found 临时重定向。
- 304 没有被修改，读取的内容为缓存
- 400 Bad Request 客户端语法错误，服务器无法理解
- 401 Unauthorized 请求需要用户认证
- 403 禁止访问(Forbidden)
- 404 没有找到要访问的内容（Not Found)
- 500 内部服务器错误
- 501 Not implemented 服务器不支持请求的功能，无法完成请求

    
## 11.MVVM模型
- Model：数据模型，可以在Model中定义数据修改和操作的业务逻辑
- view：视图，也就是UI组件，负责将数据模型转化为UI展示
- view-model：监听数据模型中数据的变化，控制视图行为，处理用户交互，简单来说就是一个同步view和model的对象，用来连接view和model
- view和model之间没有直接的关联，他们通过view-model进行交互，通过数据绑定，view和model之间是能相互影响的。


## 12.宏任务与微任务
- 首先执行主线程，主线程内的宏任务与微任务放入对应的队列当中
- 先执行微任务，再执行宏任务
- 宏任务主要是setTimeout
- 微任务主要是Promise对象


## 13.深拷贝与浅拷贝
- 浅拷贝内存地址相同，主要运用于基本数据类型
- 深拷贝内存地址不同，主要运用于引用数据类型，防止相互影响
- 浅拷贝通过直接赋值可以实现
- 深拷贝通过扩展运算符、值为对象时进行递归赋值否则简单赋值、JSON实现

  
## 14.HTML与CSS的变化
- HTML5新增：canvas/SVG、video/audio、localStorage/sessionStorage
- CSS3新增：文本效果/边框效果、2D/3D转换、transition/animation、弹性盒子、多媒体查询


## 15.性能优化
- 实现按需加载，例如vue框架中路由组件的利用required/import实现按需加载，避免打包过大加载过久，或者图片过多时进行图片的懒加载
- 优化DOM操作，例如将大量的DOM结点添加工作添加进一个DocumentFragment结点，再将该节点添加到DOM
- 减少属性的查找，例如将在一个函数中使用多次的全局对象存储为局部对象/将对象属性存储在局部对象中，避免不必要的属性查找
- 页面闪屏原因：DOM操作频繁导致页面重绘与回流增多(减少重绘和回流)
- 页面白屏原因：js操作阻塞，等待异步调用(将js操作放到页面后面)
- 图片的懒加载：
  - 预加载loading图片，src="loding"，data-src="true"
  - 判断哪些图片需要加载：当图片的可视化界面innerHeight+滚动界面scrollTop>=图片top-height
  - 通过temp.load进行图片的隐形加载
  - 加载成功后，img.src替换成data-src中的图片链接
- 减少回流与重绘
- 函数防抖：触发事件后在n秒内函数只能执行一次，如果在n秒内又触发了事件，则会重新计算函数执行时间
  - 原理：执行时清除计时器再设置定时器
  - 应用场景：搜索框输入，验证信息输入检测，窗口resize
- 函数节流：一定时间间隔内只执行一次
  - 原理：执行时若定时器存在则直接return
  - 应用场景：滚动加载、搜索联想、重复提交




## 16.移动端适配
- 开发移动设备的网站：`<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=0">`
- 辅助功能：
``` javascript
<meta content="telephone=no,email=no" name="format-detection" />
//禁止自动识别电话号码和邮箱
<meta content="yes" name="apple-mobile-web-app-capable" />
//苹果手机：会删除默认的工具栏和菜单栏，网站开启对web app程序的支持
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
//苹果手机：在web app应用下状态条（屏幕顶部条）的颜色,默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。
<meta name="apple-touch-fullscreen" content="yes" />
//苹果手机：如果把一个web app添加到了主屏幕中，那么从主屏幕中打开这个web app则全屏显示
<link rel="apple-touch-icon" href="/static/images/identity/HTML5_Badge_64.png" />
//苹果手机：将应用添加到手机主屏幕，会有一个icon可以直接进入
```
- 目前而言解决移动端开发的方案有两种
  - 响应式布局：适用于以信息发布为主，访问或交互比较少的网站，只需要维护一套代码，针对桌面和移动的代码都会被同等加载不利于网络优化(可通过模块化开发针对性加载)，JS交互操作难以做出差异(特性检测分开不同逻辑)
  - 两种设计：适用于访问或交互比较多的网站，需要维护两套代码，可同时兼容键鼠和触摸的JS操作
- 目前移动端浏览器内核：大多数移动端是-webkit-内核，使用chrome调试
- 响应式布局：
  - 多媒体查询：@media 适用不同的css代码
  - 流式布局：百分比布局，基于父元素的布局
  - 液态图片：img{max-width:100%;}
  - flex布局：弹性盒子
- 样式大小单位：
  - em单位：相对于父元素文本的尺寸为1em，根据文本尺寸设置其他css样式的大小
  - rem单位：相对于根元素文本的尺寸为1rem，根据文本尺寸设置其他css样式的大小
  - 视区宽度/高度(vw/vh)：总宽度/总高度为100，视区为window.innerWidth/Height，类似于百分比的布局
- 像素
  - 物理像素：分辨率，设备屏幕实际拥有的像素点
  - 逻辑像素：CSS像素px
  - dpr：物理像素与逻辑像素之间的比例
  - PPI：像素密度
- viewport：默认为layout viewport
  - devicePixelRatio(dpr) = 物理像素 / 独立像素
  - scale = visual viewport / layout viewport
  - layout viewport：窗口全部内容宽度，通过 document.documentElement.clientWidth 来获取
  - visual viewport：浏览器可视区域宽度，通过window.innerWidth来获取
  - ideal viewport：移动设备的屏幕宽度
  - viewport设为ideal viewport：`width=device-width`与`initial-scale=1.0`，前者解决IE的问题，后者解决iPhone、iPad的问题，同时设置发生冲突时宽度适配最大值
  - visual viewport宽度 = ideal viewport宽度  / 当前缩放值
  - 在iphone和ipad上，无论你给viewport设的宽的是多少，如果没有指定默认的缩放值，则iphone和ipad会自动计算这个缩放值，以达到当前页面不会出现横向滚动条(或者说viewport的宽度就是屏幕的宽度)的目的。
  - 动态改变meta标签：
     - document.write动态输出meta viewport标签
     - element.setAttribute()改变meta标签的content属性
- 解决方案：
  - 设计稿为750px，手机设备为375px
  - 设置根元素fontsize = 屏幕宽度/设计稿宽度*基本宽度(基本宽度建议100px)
  - 计算每个元素的rem值进行开发
  - js获取设备宽度并重置fontsize
  - 多媒体查询设置fontsize(少数分辨率适用)



## 17.mock
- 模拟后端接口假数据，进行分离开发，一旦引用mock会拦截所有的http请求
- 数据模板定义规范DTD：'name|rule'：value (属性名、生成规则、属性值) 
- 数据占位符定义规范DPD：@占位符/@占位符(参数 [, 参数])，占位符后优先调用数据模板的属性，也可调用Mock.Random中的方法
- 属性值是字符串 String
  - 'name|min-max': string：通过重复 string 生成一个字符串，重复次数大于等于 min，小于等于 max。
  - 'name|count': string：通过重复 string 生成一个字符串，重复次数等于 count。
- 属性值是数字 Number
  - 'name|+1': number：属性值自动加 1，初始值为 number。
  - 'name|min-max': number：生成一个大于等于 min、小于等于 max 的整数，属性值 number 只是用来确定类型。
  - 'name|min-max.dmin-dmax': number：生成一个浮点数，整数部分大于等于 min、小于等于 max，小数部分保留 dmin 到 dmax 位。
- 属性值是布尔型 Boolean
  - 'name|1': boolean：随机生成一个布尔值，值为 true 的概率是 1/2，值为 false 的概率同样是 1/2。
  - 'name|min-max': value：随机生成一个布尔值，值为 value 的概率是 min / (min + max)，值为 !value 的概率是 max / (min + max)。
- 属性值是对象 Object
  - 'name|count': object：从属性值 object 中随机选取 count 个属性。
  - 'name|min-max': object：从属性值 object 中随机选取 min 到 max 个属性。
- 属性值是数组 Array
  - 'name|1': array：从属性值 array 中随机选取 1 个元素，作为最终值。
  - 'name|+1': array：从属性值 array 中顺序选取 1 个元素，作为最终值。
  - 'name|min-max': array：通过重复属性值 array 生成一个新数组，重复次数大于等于 min，小于等于 max。
  - 'name|count': array：通过重复属性值 array 生成一个新数组，重复次数为 count。
- 属性值是函数 Function
  - 'name': function：执行函数 function，取其返回值作为最终的属性值，函数的上下文为属性 'name' 所在的对象。
- 属性值是正则表达式 RegExp
  - 'name': regexp：根据正则表达式 regexp 反向生成可以匹配它的字符串。用于生成自定义格式的字符串。
- Mock.mock( rurl?, rtype?, template|function( options ) )：拦截路由返回数据
  - rurl：可选，拦截的路由
  - rtype：可选，拦截的请求类型
  - template：可选，数据模板，对象或字符串形式
  - function：可选，用于生成相应数据的函数
  - options：含有请求ajax的url、type和body三个属性
- Mock.setup( settings )：配置拦截路由时的行为
  - setting：必选，配置项集合
  - timeout：可选，配置项内响应时间，'400'/'200-400'，默认值'10-100'
- Mock.Random.method([参数])：根据方法生成各种随机数据，提供部分内置方法
- Mock.Random.extend(function)：方法内可自定义扩展方法
- Mock.valid( template, data )：校验真实数据与数据模板是否匹配
- Mock.toJSONSchema( template )：将模板转化为JSON Schema
- axios-mock-adapter：设置拦截axios请求的代理api地址
``` javascript
var axios = require('axios')
var MockAdapter = require('axios-mock-adapter')
var mock = new MockAdapter(axios)
mock.onGet('/users').reply(200,{Users})
```


## 18.http头部信息
- General
  - Request URL： 请求地址
  - Request Method： 请求类型
  - Status Code： 状态码
  - Remote Address： 请求发送的目标IP地址
- Request Headers
  - Accept：浏览器可接受的MIME类型（text/html，image/*）
  - Accept-Charset：告知服务器，客户端支持哪种字符集（ISO-8859-1）
  - Accept-Encoding：浏览器能够进行解码的数据编码方式（gzip）
  - Accept-Language：浏览器支持的语言。（zh-cn）
  - Referer：请求页面url
  - Content-Type：内容类型
  - Content-Length：请求正文的长度
  - User-Agent: 包含客户端的操作系统、浏览器和其它属性等信息。
  - Host： 服务器主机地址
  - X-Requested-With： null为同步请求，XMLHttpRequest为ajax的异步请求
  - Cookie：存储信息
- Response Headers
  - Refresh：过多久刷新到哪里去
  - Date： 响应时间
  - Content-Type： 发送给客户端的实体正文的媒体类型
  - Content-length： 正文长度
  - Cache-control： 用来操作缓存的工作机制（no-cache）
  - Transfer-Encoding： 定义请求的传输编码
  - Connection： 允许客户端或服务器中任何一方关闭底层的连接双方都会要求在处理请求后关闭或者保持它们的TCP连接。（close/keep-alive）
  - Set-Cookie：设置存储信息
  



## 19.语法规范
- 组件属性传递：因为html对大小写不敏感，父组件属性需用短横线命名，子组件prop使用驼峰命名
- 组件命名：短横线命名或首字母大写命名



## 21.url解析到页面显示全过程
1. 输入网址。
2. 浏览器查找域名的IP地址。
3. 浏览器给web服务器发送一个HTTP请求
4. 网站服务的永久重定向响应
5. 浏览器跟踪重定向地址 现在，浏览器知道了要访问的正确地址，所以它会发送另一个获取请求。
6. 服务器“处理”请求，服务器接收到获取请求
7. 服务器发回一个HTTP响应
8. 浏览器开始显示HTML
9. 浏览器发送请求，以获取嵌入在HTML中的对象。


## 22.http与https的区别
- https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
- http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
- http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
- http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。


## 23.前端的SEO优化
- 重要内容优先加载，搜索引擎爬行抓取页面的顺序从上到下，从左到右
- <title>、<h1>、<strong>
- <meta>标签下的name：description/key
- 图片使用alt属性
- 网站结构扁平状有利于搜索引擎抓取
- ajax动态加载的内容搜索引擎无法识别


## 24.前后端分离
- 前端在自己的启动地址启动，通过axios请求服务器地址，后端拦截对应的服务器地址进行处理并返回信息，相当于一台主机向另一台主机进行访问的过程
- 前后端分离的MVC模式：java/php -- JavaScript、html、css -- node.js
- 前后端分离的简易MVC模式：JavaScript、html、css -- node.js(仅做返回数据库数据，不需要Model)


## 25.vue项目结构
- package/package-lock：前者锁定大版本，后者锁定小版本，npm install时根据两者安装包，锁定小版本的情况下不会对旧版本进行更新，保证项目稳定性
- node加载模块的顺序方式：文件-目录-目录下的package.json-目录下的index


## 26.登录信息存储与销毁
- 登录后根据服务端返回的信息触发vuex
- vuex中将token写入localStorage与store中
- 路由守卫检测localStorage/store存在与否，否则进行跳转进登录界面，否则next()
- 设置axios的拦截器进行拦截
拦截请求在头部加入token让服务端进行校验:
``` javascript
axios.interceptors.request.use(
  config => {
    const token = sessionStorage.getItem('token')
    if (token ) { // 判断是否存在token，如果存在的话，则每个http header都加上token
      config.headers.authorization = token  //请求头加上token
    }
    return config
  },
  err => {
    return Promise.reject(err)
  })
```
拦截响应在服务端传回对应code情况下清除token进行登出的操作:(非法或失效)
后端对token有效时间进行设置
``` javascript
axios.interceptors.response.use(
  response => {
    //拦截响应，做统一处理 
    if (response.data.code) {
      switch (response.data.code) {
        case 1002:
          store.state.isLogin = false
          router.replace({
            path: 'login',
            query: {
              redirect: router.currentRoute.fullPath
            }
          })
      }
    }
    return response
  },
  //接口错误状态处理，也就是说无响应时的处理
  error => {
    return Promise.reject(error.response.status) // 返回接口返回的错误信息
  })
``` 
- 实现登出效果可以是删除token/localStorage
- 实现长时间未点击登出效果：最后一次点击时间与当前时间的差别
``` javascript

export default{
    name: 'App',
    data (){
        return {
            lTime: new Date().getTime(), // 最后一次点击的时间
            ctTime: new Date().getTime(), //当前时间
            tOut: 10 * 60 * 1000   //超时时间10min
        } 
    },
    mounted () {
        window.setInterval(this.tTime, 1000)
    }
 
    methods:{
        clicked () {
            this.lTime = new Date().getTime()  //当界面被点击更新点击时间
        }
 
        tTime() {
            this.cTime = new Date().getTime()
            if (this.ctTime -this.lTime > tOut) {
                if(JSON.parse(sessionStorage.getItem('Login')) === true){
                    // 退出登录
                }
            }
        }
    }
}
```
- 实现离开页面保存一定时间的登录状态：页面加载中通过localStorage中设置的过期时间进行运算比较，大于过期时间则跳转回登录界面，小于过期时间则照常运行；除此之外可以设置cookie也可实现过期时间



## 27.安全
- CSRF(跨站请求伪造)：由于cookie在同源窗口间伴随着服务器的请求而发送，点击恶意网站中的服务器链接时，恶意网站不需要知道验证信息即可使用你的cookie代替你进行相应操作。token的出现可以避免使用cookie的验证方式防止CSRF


## 28.设计模式
- 单例模式：利用闭包的自调用函数确保唯一实例，一般用于全局变量
- 工厂模式：封装创建对象的具体逻辑并返回对象实例
- 策略模式：将算法的实现与算法的调用分离开，避免多重判断调用哪些算法，适用于多个判断分支
- 观察者模式：订阅者订阅发布者，某个特定事情发生的时候，发布者会通知所有的订阅者
- 模块模式：采用闭包的形式指定类想暴露的属性和方法，不会污染全局
- 构造函数模式：this指向新对象为对象添加
- 组合模式：构造函数+原型