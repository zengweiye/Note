## AJAX

### 作用

Ajax是一种异步请求数据的web开发技术，能在不重新加载页面的同时，进行异步请求加载后台数据，更改页面的数据，减少网页加载的次数，提高了用户的体验。



### 使用

#### 创建`XMLHttpRequest`对象

```js
let xhr=new XMLHttpRequest()
```



#### 向服务器发送请求

```javascript
//open('方法','地址','是否异步')
xhr.open('POST/GET','/destination','true/false')
xhr.send()
```



#### 监听`ajax`状态

```javascript
//通过ajax状态码readystate和http状态码status进行监视
xhr.onreadystatechange = function(){
	if(xhr.readystate==4 && /^(2|3)\d{2}$/.test(xhr.status)){
		//成功请求后的操作
	}
}
```

#####**`ajax`状态码

- `opend`1：已经打开(open)

- `headers_received` 2：响应头信息并已返回客户端
- `loading` 3：等待服务器返回响应内容
- `done` 4：响应主体信息已返回客户端



#####**`http`状态码**

**1**** 

信息，服务器收到请求，需要请求者继续操作

- **100**：`continue` 服务器继续其请求
- **101**：`Swtiching Protocals`切换协议，服务器根据客户端要求进行切换到更高的协议

**2****

成功，操作被成功接受并处理

- **200**：`OK` 请求成功
- **201**：`created` 已创建，成功请求并创建了新的资源
- **202**： `accepted` 已接受，已接受但并未处理完成
- **204**：`No Content`无内容，服务器成功处理但未返回内容
- .......

**3****

重定向，需要进一步的操作进行完成请求

- **300**：`Multiple Chioces`多种选择，请求的资源可包括多个位置，返回其中一个资源特征与地址的列表用于客户端选择
- **301**：`Moved Permanently` 永久移动，返回信息包括新URI，浏览器以后会自动重定向到新URI
- **302**：`Found` 临时重定向。
- ......

**4****

客户端错误，请求语法错误或无法完成请求

- **400**：`Bad Request` 客户端语法错误，服务器无法理解
- **401**：`Unauthorized` 请求需要用户认证
- **403**：`Forbidden`服务器理解请求但拒绝执行请求
- **404**：`Not Found` 服务器无法找到资源或网页
- ......

**5****

服务器错误，服务器在处理请求过程中发生错误

- **500**：`Internal Server Error` 服务器内部发生错误
- **501**：`Not implemented` 服务器不支持请求的功能，无法完成请求
- **502**：`Bad Gateway` 作为网关或代理工作的服务器尝试执行请求时，从远程服务器收到无效的响应。
- ......



#### 服务器响应处理

**同步**

```javascript
xhr.open("GET","info.txt",false);  
let result=xhr.responseText
```

**异步**

```javascript
xhr.onreadystatechange=function(){
	if(xhr.readystate==4 && xhr.status==200){
		let result= xhr.responseText
	}
}
```



### 客户端与服务器之间的信息传递

#### 客户端->服务器

- 问号传参，通过在`url`后面加入参数进行传递
- 设置请求头，`xhr.setRequestHeader([key],[value])`
- 设置请求主体，`xhr.send(请求主体信息)`



#### 服务器->客户端

- 响应头
- 响应主体



### get与post区别

1. `get`给的少，拿的多，`post`给的多，拿的少，但都将信息传到服务器，也能从服务器拿到信息。
2. `get`传递到服务器一般采取问号传参，因为`url`有长度限制，但`get`传递的信息少。
3. `post`传输信息到服务器一般使用设置请求主体
4. `post`理论上不存在大小限制。
5. `get`会产生缓存，因为浏览器会将同一地址进行缓存，可能服务器数据不同但浏览器缓存数据没改变，所以可以通过在`url`上增加时间戳或者随机数进行取消缓存。
6. `get`进行问号传参可能被劫持，所以并没有`post`安全。

