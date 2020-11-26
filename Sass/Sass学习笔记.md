## `Sass`学习笔记

### 变量声明

通过`$`符号进行声明变量。

```css
$high-light: #F90
```

相当于定义了一个值为`#F90`的变量`$high-light`，这个变量在其他代码中能被引用。

如果变量定义在规则块外，则能被任意规则块调用，而如果定义在某一规则块内，则只能在里面使用，相当于作用域。



### 嵌套规则

在`Sass`中能使用嵌套，做到代码的简洁

```css
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```



在使用伪类时需要注意，需要通过`&`来使用父选择器进行

```css
/* 错误 */
article a {
  color: blue;
  :hover { color: red }
}
/* 正确 */
article a {
  color: blue;
  &:hover { color: red }
}
/* 编译后 */
article a { color: blue }
article a:hover { color: red }
```



**群组选择器嵌套**

```css
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
/* 编译后 */
.container h1, .container h2, .container h3 { margin-bottom: .8em }
```



**`>`,`+`和`~`**

```css
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
/* 编译后 */
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```



**嵌套属性**

对于通过`-`进行连接的子属性，可以通过根属性后接`:{}`进行分割嵌套

```css
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}

/* 编译后 */
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```



### 静默注释

```css
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```



### 混合器

混合器中不仅可以包含属性，也可以包含`css`规则，包含选择器和选择器中的属性。

```css
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
```

当一个包含`css`规则的混合器通过`@include`包含在一个父规则中时，在混合器中的规则最终会生成父规则中的嵌套规则。

```css
ul.plain {
  color: #444;
  @include no-bullets;
}
/* 编译后 */
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

**混合器传参**

```css
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
  @include link-colors(blue, red, green);
}

/* 编译后 */

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```

**默认参数**

通过`$name: value`设置默认参数

```css
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```



### 选择器继承

通过`@extend`语法进行选择器的继承

```css
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

`.seriousError`将会继承样式表中任何位置处为`.error`定义的所有样式。并且会继承所有与`.error`有关的组合选择器

```css
//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
```

