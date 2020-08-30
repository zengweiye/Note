## 选择器

### 选择器特异性

内联样式>ID选择器>类选择器>分组选择器>通配选择器



### 选择器重复

当一个选择器定义出现重复时，相同的属性将被覆盖，而不同的属性将会被添加。



### 通配选择器

**格式**

```css
/* *{style} */
* {color: red;}
```



### 分组选择器

**格式**

```css
/*tag1,tag2,...{style}*/
h2, p {color: gray;}
```



### 类选择器

**格式**

```css
/* .className{style} */
.warning {font-weight: bold;}
```



#### 多类选择器

**格式**

```css
/* .className1.className2{style} */
/* 同时存在两个类名时生效 */
.warning.urgent {background: silver;}
```



### ID选择器

ID选择器无法想类选择器一样进行联合使用，正常来说，一个文档中只应该被使用一次且仅有一次

**格式**

```css
/* #IDName{style} */
#lead-para {font-weight: bold;}
```



### 属性选择器

#### 简单属性选择器

**格式**

```css
/* Tag[attribute]{style} */
h1[class] {color: silver;}
```

用于选择带有某种属性的便签。



#### 基于准确值的属性选择器

**格式**

```css
/* Tag[attribute="value"]{style} */
a[href="http://www.css-discuss.org/about.html"] {font-weight: bold;}
```

用于选择带有某种属性具体值的便签。



#### 基于部分值的属性选择器

**类型**

- `[foo~="bar"]`
  - 选择所有带有`foo`属性、且`foo`属性被空白分隔的单词列表中含有单词`bar`的元素。
- `[foo*="bar"]`
  - 选择所有带有`foo`属性、且`foo`属性值中含有子串`bar`的元素。
- `[foo^="bar"]`
  - 选择所有带有`foo`属性、且`foo`属性值以`bar`开头的元素。
- `[foo$="bar"]`
  - 选择所有带有`foo`属性、且`foo`属性值以`bar`结束的元素。
- `[foo&#124;="bar"]`
  - 选择所有带有`foo`属性、且`foo`属性值以`bar`开头后接一个短线（`U+002D`）或者属性值是`bar`的元素。



**格式**

```css
/* Tag[attribute symbol="value"]{style} */
img[src|="figure"] {border: 1px solid gray;}
```



#### 大小写忽略符

**格式**

```css
/* Tag[attribute symbol="value" i]{style} */
a[href$='.PDF' i]{color: red;}
```



###文档结构

#### 后代选择器

其实本质就是选择器的嵌套。根据文档中标签的父子关系，进行嵌套。但这种选择器存在一种问题，就是无论子标签嵌入得有多深入，都会被更改样式。

**格式**

```css
/* parentTag childTag{style} */
h1 em {color: gray;}
```

相当于改变 `h1` 中的 `em`  的样式。



#### 选择子元素

选择子元素能够避免产生后代选择器中的问题。它只会对第一个标签中的子元素进行更改，而不会继续深入，也不会对第二个相同标签进行更改。

**格式**

```css
/* parentTag > childTag{style} */
h1 > strong {color: red;}
```



####选择相邻兄弟元素

选择兄弟元素能够针对第一个元素的兄弟元素进行更改。

**格式**

```css
/* broTag1+broTag2{style} */
h1 + p {margin-top: 0;}
```

上述例子表示修改紧跟在`h1`后的`p`样式



#### 选择跟随兄弟元素

针对同一父元素下的跟随在第一个元素后的所有兄弟元素进行更改，不限于一个，也不限于必须相邻

**格式**

```css
/* broTag1~broTag2{style} */
h2 ~ ol {font-style: italic;}
```



### 伪类选择器

伪类选择器是CSS中已经定义好的选择器，不能随便取名

通过链式来进行更改特殊类型的样式。

#### 组合伪类

**格式**

```css
/* Tag:selector1:selector2... {style} */
a:link:hover {color: red;} 
a:visited:hover {color: maroon;}
```

伪类之间可以进行组合，但不能是互斥的。



#### 结构性伪类

#####	选择根元素

**格式**

```css
/* :roor{style} */
:root {border: 10px dotted gray;} 
```

`:root`表示选择文档的根元素，而在HTML中，可以直接用`html`元素进行取代，因为HTML的根元素永远都是`html`元素



##### 选择空元素

**格式**

```css
/* tag:empty{style} */
p:empty {display: none;}
```

选择没有子节点的元素，但带有换行符或空格的文本节点并不会被选择



##### 选择唯一子元素

**格式**

```css
/* tag:only-child{style} */
img:only-child {border: 1px solid black;}
```

选择某个父元素下唯一的子元素，但用的是子元素，并非父元素。



##### 选择第一个和最后一个子元素

**格式**

```css
/* tag:first-child{style} */
/* tag:last-child{style} */
p:first-child {font-weight: bold;}  
p:last-child {font-weight: bold;}  
```

选择元素的第一个子元素（最后一个子元素）。

两者一起使用可以合并成：`:only-child`意为仅有一个

`:first-child:last-child`-->`only-child`



#####选择第一个和最后一个特定类型元素

**格式**

```css
/* tag:first-of-type{style} */
/* tag:last-of-type{style} */
table:first-of-type {border-top: 2px solid gray;}
table:last-of-type {border-top: 2px solid gray;}
```

选择某个具有相应类型子元素的父元素中的子元素。而不是整个文档中的第一个元素。

格式中的`type`指的是`tag`的类型。

两者一起使用时可以合并成`:only-of-type`意为仅有一个

`:first-of-type:last-of-type`-->`only-of-type`





##### 选择每第N个元素

**格式**

```css
/* tag:nth-child(n) */
p:nth-child(1) {font-weight: bold;} 
tr:nth-child(2n+1) {background: silver;}/* 通过表达式进行选择 */
tr:nth-child(odd) {background: silver;}/* 也可以通过关键字 "odd"/"even" */
```

选择元素的第n个子元素。

同理也有选择倒数第N个元素`:nth-last-child()`



#### 动态伪类

动态伪类能够随页面状态改变

- link：用来定义链接未被访问的样式
- visited：用来定义链接已经被访问过的样式（默认状态下是跟踪了用户的行为）
- hover：用来定义用户用**鼠标**划过对应的元素，但是未激活显示的样式
- focus：用来定义一个元素本身具备焦点（接受键盘、鼠标、form的输入等）之后，显示的样式
- active：用来定义用户按下鼠标后，但是并未离开时候的样式，通常是左侧的鼠标

**PS**

- 定义顺序：按照link-visited-hover-active的顺序设置对应的样式，才会有效

- 对于div是没有focus的行为的，因为一个div没法用鼠标获得焦点，但是可以通过设置div的`tabIndex`

- 对于hover，移动端没有hover，hover和active会合并在一起，对于键盘设备，hover这个状态不会存在



#### UI状态伪类

根据UI的状态来变更样式

![image-20200507224805003](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200507224805003.png)



#### :target伪类

URL 中包含一个片段标识码，在 CSS 中叫做 *target*，可使用`:target`伪类为其添加样式。

`:target`选择器可用于当前活动的target元素的样式。



#### 否定伪类

`:not()`选择器，括号里面填写简单选择器

**格式**

```css
/* :not(selector) */
:not(p){style}
```



### 伪元素选择器

##### 样式化第一个字母

**格式**

```css
/* tag::first-letter{style} */
p::first-letter {color: red;}
```



##### 样式化第一行

**格式**

```css
/* tag::first-line{style} */
p::first-letter {color: red;}
```





##### 限制

上述两个选择器只能用于块级元素，不能用于行内元素，而且属性有限制。![image-20200507230155857](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200507230155857.png)



##### before与after

**格式**

```css
/* tag::before{style} */
/* tag::after{style} */
h2::before {content: "]]"; color: silver;}
body::after {content: "The End.";}
```

在标签的前后加入`content`的内容

