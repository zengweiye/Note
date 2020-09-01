## Stream流

### 什么是流

流是驱动`Nodejs`应用的基础概念之一，是一种处理读写文件，网络通信或任意端到端信息交换的有效方式。



### 流的作用

不需要将所有数据加载到内存，而是将其作为连续流的数据块，减少了内存的占用

**内存效率**：不需要加载大量数据就可以处理

**时间效率**：一旦有数据就能够处理，不需要全部加载完成



### 流的类型

1. **可读流**

   可以读取数据的流，例如`fs.createReadStream`和`request`

2. **可写流**

   可以写入数据的流，例如`fs.createWriteStream`和`reponse`

3. **双工流**

   即可读又可写的流，例如`net.socket`

4. **转换流**

   在数据写入或读取时修改或转换数据的流。例如在文件压缩时，可以向文件写入数据，也可从文件读取数据



### 创建可读流

**可读流事件与方法**

1. **Events**

   `data`,` end`, `error`, `close`, `readable`

2. **Functions**

   `pipe(),` `unpipe()`

   `read()`, `unshift()`, `resume()`

   `pause()`, `isPaused()`

   `setEncoding()`

```javascript
//创建可读流
const Stream = require('stream')
const readableStream = new Stream.Readable()
//填充内容
readableStream.push('one')
readableStream.push('two')
//填充结束
readableStream.push(null)
//输出
readableStream.pipe(process.stdout)
```



### 读取模式

#### **`flowing`模式**

流模式，数据从底层系统进行获取，可以通过`EventEmitter`接口使用事件提供给应用程序

`fs.createReadStream`提供可读流，一开始是静止状态，一旦监听`data`事件并绑定回调函数，就会开始流动。读取数据块，直到没有数据块可以读取时触发`end`事件，表示结束。



#### **`pause`模式**

暂停模式，必须显示调用`stream.read()`来从流中读取数据

```javascript
readableStream.on('readable', function() {
    while ((chunk=readableStream.read()) != null) {
        data += chunk;
    }
});

readableStream.on('end', function() {
    console.log(data)
});
```

`read()` 函数从内部缓冲区读取一些数据并返回。当没有要读取的内容时，它返回 `null`

`readable`事件是在可以从流中读取数据块时发出的。



#### 模式切换

其实所有`Readable`数据流都是以`pause`模式开始的，但可以通过几种方式进行切换到`flowing`模式

1. 添加`data`事件处理器
2. 调用`stream.resume`方法
3. 调用`stream.pipe()`方法发送数据到`writable`

也可以通过下面几种模式切换回来

1. 如果没有管道目标，调用`stream.pause()`
2. 如果有管道目标，通过`stream.unpipe()`进行删除管道

####重要概念

除非提供了一种用于消费或忽略该数据的机制，否则`Readable` 将不会生成数据。如果消费机制被禁用或取消，`Readable`将*尝试*停止生成数据。 添加一个`readable` 事件处理程序会自动使流停止流动，并通过`readable.read()`消费数据。如果删除了`readable`事件处理程序，那么如果存在`data`事件处理程序，则流就会再次开始流动

### 创建可写流

**可写流事件和方法**

1. **Events**

   `drain`（表示能接收更多数据）, `finish`, `error`, `close`, `pipe/unpipe`

2. **Functions**

   `write()`, `end()`, `cork()`, `uncork()`, `setDefaultEncoding()`

从文件中获取数据

```javascript
var fs = require('fs');
var readableStream = fs.createReadStream('file1.txt');
var writableStream = fs.createWriteStream('file2.txt');

readableStream.setEncoding('utf8');

readableStream.on('data', function(chunk) {
    writableStream.write(chunk);
});
```

从可读流中获取数据

```javascript
readableStream.pipe(writableStream)

readableStream.push('ping!')
readableStream.push('pong!')

writableStream.end()
```



### 创建管道

管道是一种机制，是将一个流的输出作为另一流的输入。它通常用于从一个流中获取数据并将该流的输出传递到另外的流。管道操作没有限制，换句话说，管道用于分步骤处理流数据。

```javascript
const { pipeline } = require('stream');
const fs = require('fs');
const zlib = require('zlib');

// 使用 pipeline API 轻松管理多个管道流，并且在管道全部完成时得到通知
// 一个用来高效压缩超大视频文件的管道

pipeline(
  fs.createReadStream('The.Matrix.1080p.mkv'),
  zlib.createGzip(),
  fs.createWriteStream('The.Matrix.1080p.mkv.gz'),
  (err) => {
    if (err) {
      console.error('Pipeline failed', err);
    } else {
      console.log('Pipeline succeeded');
    }
  }
);
```



### `pipe`与`pipeline`对比

```javascript
const fs = require('fs');
const r = fs.createReadStream('file.txt');
const z = zlib.createGzip();
const w = fs.createWriteStream('file.txt.gz');
r.pipe(z).pipe(w);


const { pipeline } = require('stream');
const fs = require('fs');
const zlib = require('zlib');
pipeline(
  fs.createReadStream('archive.tar'),
  zlib.createGzip(),
  fs.createWriteStream('archive.tar.gz'),
  (err) => {
    if (err) {
      console.error('Pipeline failed.', err);
    } else {
      console.log('Pipeline succeeded.');
    }
  }
);
```

`pipe`在响应即将退出或客户端关闭，则可读流不会关闭或销毁，会造成内存泄漏

而`pipeline`中则会关闭所有其他流，确保没有内存泄漏。