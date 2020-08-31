## Promise

### `Promise`作用

避免回调地狱。

在进行异步调用时，如果回调函数一旦进行嵌套，代码就会十分难以阅读分析，为了避免这个情况，可以使用`Promise`进行替代回调函数嵌套



### `Promise`标准

1. Promise对象初始状态为 `Pending`，在被 `resolve` 或 `reject` 时，状态变为 `Fulfilled` 或 `Rejected`，而且状态一旦变更，不可再次变化。
2. `resolve`接收成功的数据，`reject`接收失败或错误的数据
3. Promise对象必须有一个 `then` 方法，且只接受两个可函数参数 `onFulfilled`、`onRejected`



###`then()`

`Promise`通过`then()`方法进行添加回调函数，`then`第一个参数为处理`resolved`状态的回调函数，第二个参数为处理`rejected`状态的函数（可选）。返回结果为一个`Promise`对象。而且只有回调函数被处理完后才会进行链式的下一个函数。



### `catch()`

相当于`then(null,rejection)`，用于错误时的回调函数。`Promise`对象的错误具有冒泡性质，会一直在链式调用中传递。



### `finally()`

不管结果如何都会调用的函数



### `all()`

对`Promise`进行批处理，将一组`Promise`对象以数组形式作为参数打包成一个。只要有一个为`rejected`，便返回`rejected`

```javascript
function promiseAll(promises) {
  return new Promise(function(resolve, reject) {
    if (!isArray(promises)) {
      return reject(new TypeError('arguments must be an array'));
    }
    var resolvedCounter = 0;
    var promiseNum = promises.length;
    var resolvedValues = new Array(promiseNum);
    for (var i = 0; i < promiseNum; i++) {
      (function(i) {
        Promise.resolve(promises[i]).then(function(value) {
          resolvedCounter++
          resolvedValues[i] = value
          if (resolvedCounter == promiseNum) {
            return resolve(resolvedValues)
          }
        }, function(reason) {
          return reject(reason)
        })
      })(i)
    }
  })
}
```



### `race()`

对`Promise`进行批处理，将多个`Promise`对象打包成一个，只要其中一个`Promise`对象改变状态，就返回状态值.

```javascript
 Promise.race = function(promises) {
    return new Promise(function(resolve, reject) {
      for (let i = 0; i < promises.length; i++) {
        Promise.resolve(promises[i]).then(function(value) {
          return resolve(value)
        }, function(reason) {
          return reject(reason)
        })
      }
    })
  }
```



### `resolve()`

将对象转化为状态值为`resolve` 的 `Promise`对象



###`reject()`

将对象转化为状态值为`rejected `的 `Promise`对象