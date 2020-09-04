## `http`模块

### `http`服务器搭建

通过`createServer`进行搭建

```javascript
const http = require('http')
http.createServer(function(req,res){\
    let chunk=''
    // 可以通过监听request的data事件进行处理接收到的数据
    // request以及response是流，可以通过流处理进行处理数据
    req.on('data',function(data){
        chunk+=data
        console.log(data)
    })
    req.on('end',function(){
        res.write(chunk)
    })
	res.writeHead('200',{'content-Type' : 'text/plain'})
	res.write('hello world')
	res.end()
}).listen(8081)
```



### 发出请求

#### `http.get`

通过`get`发送请求，第一个参数为请求设置，可以设置地址和路径；第二个参数为回调函数，用来处理`response`数据

```javascript
http.get(options,callback)
```



#### `http.request`

`request`同样是请求，但他的请求设置更加多样，第二个参数仍为`callback`函数

**options设置**

- `host`：`HTTP`请求所发往的域名或者`IP`地址，默认是`localhost`。

- `hostname`：该属性会被`url.parse()`解析，优先级高于`host`。

- `port`：远程服务器的端口，默认是80。

- `localAddress`：本地网络接口。

- `socketPath`：`Unix`网络套接字，格式为`host:port`或者`socketPath`。

- `method`：指定HTTP请求的方法，格式为字符串，默认为GET。

- `path`：指定HTTP请求的路径，默认为根路径（/）。可以在这个属性里面，指定查询字符串，比如`/index.html?page=12`。如果这个属性里面包含非法字符（比如空格），就会抛出一个错误。

- `headers`：一个对象，包含了`HTTP`请求的头信息。

- `auth`：一个代表HTTP基本认证的字符串`user:password`。

- `agent`：控制缓存行为，如果HTTP请求使用了`agent`，则HTTP请求默认为

  ```plaintext
  Connection: keep-alive
  ```

  ，它的可能值如下：

  - `undefined`（默认）：对当前`host`和`port`，使用全局`Agent`。
  - `Agent`：一个对象，会传入`agent`属性。
  - `false`：不缓存连接，默认HTTP请求为`Connection: close`。

- `keepAlive`：一个布尔值，表示是否保留socket供未来其他请求使用，默认等于`false`。

- `keepAliveMsecs`：一个整数，当使用`KeepAlive`的时候，设置多久发送一个TCP `KeepAlive`包，使得连接不要被关闭。默认等于1000，只有`keepAlive`设为true的时候，该设置才有意义。

**回调函数**

回调函数返回`response`的数据，并且只会触发一次

`http.request`返回一个`http.ClientRequest`类的实例，是一个可写数据流，可以直接写入数据到对象中。



