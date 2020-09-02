## webpack相关

### webpack功能和原理

1. 代码转换：`ts`转成`js`，`scss`转成`css`等
2. 文件优化：压缩`js`, `css`, `html`等代码，压缩图片
3. 代码分割：提取多个页面的公共代码，提取首屏不需要执行的代码让其异步加载
4. 模块合并：将各个模块分类合并成一个文件
5. 自动刷新：监听代码的变化，自动构建，刷新浏览器
6. 代码校验：检测代码是否符合标准，以及单元测试是否通过
7. 自动发布：更新代码后，自动构建线上发布代码并传输到线上。



### webpack构建过程

1. 从`entry`里配置的`moudle`开始递归解析`entry`依赖的所有`moudle`
2. 每找到一个`moudle`就会根据配置的`loader`去找相应的转换规则
3. 对`moudle`进行转换后，在解析出当前`moudle`依赖的`moudle`
4. 这些模块以`entry`为单位分组，一个`entry`和其他依赖的`moudle`将分到同一组`chunk`
5. 最后`webpack`将所有`chunk`转换为文件输出
6. 流程中会在恰当的时候执行`plugin`里定义的逻辑



### webpack与`gulp`，`grunt`区别

`gulp`只是一个`task runner`，无法做到模块化

webpack是一个模块化打包工具，能递归打包文件中所有模块，最终生成打包后的文件。



### `entry`与`output`

`entry`：构建项目的起点

`output`：输出打包好的代码的位置以及命名



### `loader`与`plugins`

`loader`：用于转换某一类型的文件，并且引入到打包出的文件中

`plugins`：能用于打包优化，资源管理和注入环境变量。



### `bundle`,`chunk`,`moudle`

`bundle`：打包出来的文件

`chunk`：进行模块的依赖分析时代码分割出来的代码块

`moudle`：开发中的单个模块

