## 盒模型

### 盒模型要素

**从内到外**：

1. `content`：内容
2. `padding`：内边距
3. `border`：边框
4. `margin`：外边距



### content

盒模型的主要内容，主要显示区域



### padding

补充`content`边缘部分，与`content`内容不可分，颜色即为`content`背景色。



### border

可以进行添加外部的边框。



### margin

与其他组件的外部距离。

####负值

`margin`中允许值为负数。

当`margin`为负值时有两种情况：

1. `width`为`auto`时，`left`，`right`为负值，增加`width`的数值，`top`为负值则上移，`bottom`为负值外表不做变化，但实际`content`内容减少，下方元素能上移到元素内
2. `width`为实际值，`left`为负值，向左偏移;`right`为负值时实际`content`减少，右方元素能进入。`top`,`bottom`和第一条一致。

原因：`left`，`top`以`margin`为边界，而`right` ，`bottom`以`content`为边界。当`width`为`auto`时，要满足`auto`规则，而`top`，`bottom`不受`atuo`控制。



#### 外边距合并

在CSS中，两个或多个**毗邻**的**普通流**中的盒子（可能是父子元素，也可能是兄弟元素）在**垂直方向**上的**外边距**会发生叠加，这种形成的外边距称之为外边距叠加

**普通流**：只要不是 `float`、`absolutely positioned` 和 `root element` 时就是普通流。



### box-sizing

`content-box`：标准盒模型，`width`和`height`只包括`content`的宽和高，不包括`padding`，`border`，`margin`。

`border-box`：IE盒模型，`width`和`height`只包括`content`,`padding`，`border`的宽和高之和，不包括`margin`。

我们可以通过`border-box`属性直接得出一个除去外边距的合适大小的盒模型，更加方便



### auto

在盒模型中，有4个属性可以设定为`auto`，分别为`width`，`height`，`margin-left`，`margin-right`。

当设置成`auto`后，会自动填充至父元素宽度的大小。其中`width`优先级比`margin`高



