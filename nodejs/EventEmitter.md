## `EventEmitter`

### `EventEmitter介绍`

`EventEmitter`是`nodejs`中一个实现观察者模式的类，主要功能是监听和发射消息，用于处理多模块交互问题



### `EventEmitter`原理

观察者模式管理消息分发的一种方式，这种模式中 ，发布消息的一方不需要知道这个消息会给谁，而订阅一方也无需知道消息的来源。

`this.events`维护一个信号对应函数的列表,通过这个索引，可以对这个信号进行增、删等操作，你只需要按照这个信号执行对象的回调函数；