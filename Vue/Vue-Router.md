## Vue-Router

### 路由守卫

1. 全局前置守卫`router.beforeEach`

   在到达前触发

2. 全局解析守卫`router.beforeResolve`

   在导航被确认前，同时所有组件内守卫和异步路由解析之后触发

3. 全局后置守卫`router.afterEach`

   在到达后触发且不存在`next`

4. 路由独享守卫`beforeEnter`

   在路由配置上定义，在到达前触发

5. 组件内守卫

   1. `beforeRouteEnter`：在到达前触发，不能调用`this`对象，因为还没有进行创建实例。但是能通过传一个回调给`next`进行访问实例，实例接受回调的生命周期为`mounted`之后。
   2. `beforeRouteUpdate`：当前路由改变，但该组件被复用时触发。可以访问`this`
   3. `beforeRouteLeave`：导航离开组件时触发，可以调用`this`



### 导航解析流程

1. 导航被触发
2. 在失活的组件里调用`beforeRouteLeave`
3. 调用全局的`beforeEach`
4. 在重用的路由里调用`beforeRouteUpdate`
5. 在路由配置里调用`beforeEnter`
6. 解析异步路由组件
7. 在被激活的组件里调用`beforeRouteEnter`
8. 调用全局的`beforeResolve`
9. 导航被确认
10. 调用全局`afterEach`
11. 触发`DOM`更新
12. 调用`beforeRouteEnter`中的`next`的回调函数。创建好的实例将作为回调函数的参数传入。



### 路由传参

1. 直接在动态路由上传参

   ```javascript
   {
   	path:'/path/:data',
   	name:'route',
   	component:path
   }
   this.$route.params.data
   ```

2. 通过`params`传参

   ```javascript
   this.$router.push({
   	name:'path',
   	params:{
   		data:data
   	}
   })
   this.$route.params.data
   ```

   通过这种方式传参，数据不会再地址栏显示，但刷新后数据消失

3. 通过`query`传参

   ```javascript
   this.$router.push({
   	name:'path',
   	query:{
   		data:data
   	}
   })
   this.route.query.data
   ```

   与第二种方式相反，数据不会因为刷新消失，但会显示在地址栏

