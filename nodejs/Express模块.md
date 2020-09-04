## Express模块

### 路由

路由表示应用程序端点 (URI) 的定义以及端点响应客户机请求的方式

```javascript
const express = require('express')
const app = express()
app.get('/',function(req,res){
	res.send('hello world')
})
```



### 路由方法

路由方法有：`get`、`post`、`put`、`head`、`delete`、`options`、`trace`、`copy`、`lock`、`mkcol`、`move`、`purge`、`propfind`、`proppatch`、`unlock`、`report`、`mkactivity`、`checkout`、`merge`、`m-search`、`notify`、`subscribe`、`unsubscribe`、`patch`、`search` 和 `connect`。



### 路由路径

路由路径与请求方法相结合，用于定义可以在其中提出请求的端点。路由路径可以是字符串、字符串模式或正则表达式。

**字符串**

```javascript
app.get('/about', function (req, res) {
  res.send('about');
});
```

**字符串模式**

```javascript
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});
```

匹配`abcd`，`acd`

**正则表达式**

```javascript
app.get(/a/, function(req, res) {
  res.send('/a/');
});
```

匹配所有带有`a`的路由路径



### 路由处理程序

可以通过一个或者多个回调函数进行处理路由

```javascript
//一个
app.get('/example/a', function (req, res) {
  res.send('Hello from A!');
});
//多个
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});
//数组
app.get('/example/c', [cb0, cb1, cb2]);
```



### `route()`

可以通过`route()`进行创建多个路由处理程序

```javascript
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });
```



### 模块化

通过使用`Router()`进行模块化。并在应用程序装入路由器模块`use(path, moudule)`



### 应用层中间件

通过`use()`将应用层中间件绑定到应用程序对象的实例

1. 没有路径参数时，每次收到请求都会执行该函数

   ```javascript
   app.use(function (req, res, next) {
     console.log('Time:', Date.now());
     next();
   });
   ```

2. 存在路径时，只有跳转路径才会执行该参数

   ```javascript
   app.use('/user/:id', function (req, res, next) {
     console.log('Request Type:', req.method);
     next();
   });
   ```



### 路由层中间件

路由层中间件与应用层基本相同，不同之处在于它绑定到`express.Router()`实例，并且需要挂载到应用层上`app.use('/', router);`



### 错误处理中间件

错误处理中间件函数的定义方式与其他中间件函数基本相同，差别在于错误处理函数有四个自变量而不是三个，专门具有特征符 `(err, req, res, next)`：