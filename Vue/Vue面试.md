## `Vue`面试

### `MVVM`的理解

`Model`：数据模型，可以在`Model`中定义数据修改和操作的业务逻辑

`view`：视图，也就是`UI`组件，负责将数据模型转化为`UI`展示

`view-model`：监听数据模型中数据的变化，控制视图行为，处理用户交互，简单来说就是一个同步`view`和`model`的对象，用来连接`view`和`model`

`view`和`model`之间没有直接的关联，他们通过 `view-model`进行交互，通过数据绑定，`view`和`model`之间是能相互影响的。



### 生命周期

`beforeCreate`：数据观测和初始化事件未开始

`created`：完成数据观测，属性和方法的计算，初始化事件，`$el`属性未显示

`beforeMount`：`render`函数被调用，实例完成编译模板，并把数据和模板生成`HTML`

`mounted`：用编译好的`HTML`来替换`el`属性指向的`DOM`对象，完成模板中的`HTML`渲染到页面

`beforeUpdate`：能进一步更改状态，不触发附加的重渲染过程

`updated`：避免在此阶段进行更改状态，避免更新无线循环

`beforeDestory`：在此状态仍然可以调用实例

`destoryed`：实例被销毁，监听事件也被销毁

`activated`：切换到当前组件时调用

`deactivated`：切换组件后调用



### 双向绑定原理

数据劫持结合发布者-订阅者模式

主要是通过`Object.defineProperty()`来劫持每个属性的`getter`、`setter`，在数据变动时发布消息给订阅者(`watcher`)，触发响应的监听回调。

**`getter`、`setter`从哪来？**

每个`js`对象传给`vue`实例当作它的`data`选项时，`vue`都会遍历他的属性，用`Object.defineProperty`将他们转为`getter/setter`，通过`getter/setter`，`vue`就能在属性被修改和访问时响应。

**双向绑定的具体实现**

实现监听器`Observer`：对每个属性进行遍历，利用`Object.defineProperty()`对每个属性加上`setter`,`getter`。加上之后，就能在修改属性时触发`setter`，这样就能监听到数据变化。

实现解析器`compile`：解析`vue`模板指令，将模板变量替换成数据，初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听函数的订阅者`watcher`，一旦数据变动，收到通知，就调用更新函数进行数据更新。

实现订阅者`watcher`：订阅者`watcher`是监听器`observer`和解析器`compile`之间的桥梁，收到监听器`observer`的数据变化后，通知解析器`complie`，触发其中相应的更新函数。

实现订阅器`dep`：用来收集订阅者`watcher`，对监听器`observer`和订阅者`watcher`统一管理



### 无法监测到数组变化原理

在实现监听器时，使用了`Object.defineProperty()`来给每一个属性添加上`getter，setter`，而当我们直接通过索引改变数组时，视图`view`却不会更新，因为在通过索引直接改变数组的值时，新增的值并没有通过`Object.defineProperty()`进行增加`setter`,`getter`,所以只会改变数组内容，却不会在视图`view`中更新。

**如何解决**

通过**变异方法**，和普通的数组方法名称一样，但是都会在更改数组后进行遍历增加`setter`,`getter`。

通过`set`方法添加元素。（同样方法也能用在对象上）



### `key`的作用

`key`的作用与`vue`的虚拟`DOM`算法中的`diff`算法相关，使用`key`能更加高效。使用`key`后就能更快地拿到`oldNode`中对应的`Vnode`节点，而且也更准确，因为设置了`key`后，实现的是`map`的映射来获取，而不是遍历。



### 单向数据流绑定

父级`prop`的更新会向下流动到子组件，而反过来则不行，当子组件需要修改数据时，`vue`会警告，所以在修改`props`数据时，可以先在子组件将`props`中的值赋到新建的值上，再进行操作。



### `vue`组件之间的通信

1. `props`与`$emit`

   通过`props`将值传到子组件，用`$emit`将值传回父组件

2. `ref`与`$parent/$children`

   通过`ref`指向组件实例，通过`refs`获取组件数据

3. `EventBus`

   通过建立一个空的`vue`实例作为中央事件总线，用它来触发和监听事件，达到通信的效果

4. `$attrs`/`$listeners`

   `$attrs`包含了父作用域所有不被`prop`所识别的特性绑定，可以通过`v-bind:$attrs`传入组件内部

   `$listeners`包含了父作用域所有的`v-on`事件监听器，可以通过`v-on=$listeners`传入

5. `Vuex`



### `computed`与`watch`

`computed`：计算属性，依赖其他值，并且有缓存，只有依赖的值改变后才会进行重新计算。不支持异步，异步操作时无法监听数据变化

`watch`：更多的是用于观察，可以用于监听回调，当数据变化执行异步或开销较大的操作时，这个方法最有用。支持异步操作



### 父组件监听子组件生命周期

1. 通过`$emit`
2. 通过`@hook`



### 路由实现

1. `hash`模式：在浏览器中加`#`，`#`以及后面的字符称之为`hash`，用`window.location.hash`读取

   虽然在`URL`中，但不会包括在`HTTP`请求中，对后端没有影响，不会重新加载页面

2. `history`模式：利用`pushState`和`replaceState`对历史记录栈进行修改，但不会立刻跳转，只是更改`URL`栏；以及`popState`事件，但前面两种函数都不会触发这个事件，这个事件只会在浏览器某种行为后触发。



### SPA单页面

`SPA`单页面，仅在页面初始化时就加载完了相应的`HTML`，`JS`，`CSS`。一旦页面加载完成，不在会因为用户的操作进行加载和跳转，而是利用路由机制进行实现`HTML`的变换。

**优点**

1. 用户体验好，快，内容改变不必要重新加载，避免了不必要的渲染和跳转。
2. SPA相对对服务器压力小
3. 前后端职责分离，前端负责交互逻辑，后端负责数据处理

**缺点**

1. 初次加载时耗时长
2. 路由管理：需要自己建立堆栈进行管理前进后退
3. `SEO`难度大。



### `data`为什么是函数

`data`如果不是函数而是对象的话，会因为其他实例改变了`data`的数据而导致当前`data`数据的改变。而函数则不会有这种情况，这是因为`data`是对象的话，引用的是同一个内存地址。



### `render`与`template`区别

`render`性能比`template`性能更好，因为`template`会被编译成`AST`抽象语法树，在经过`generate`得到`render`函数。

