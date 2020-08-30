## 语句

### if语句

```javascript
if (condition) 
    statement1 
else 
    statement2 
```

condition（条件）可以是任意表达式；而且对这个表达式求值的结果不一定是布尔值。 ECMAScript 会自动调用 Boolean()转换函数将这个表达式的结果转换为一个布尔值。



### do-while语句

do-while 语句是一种后测试循环语句，即只有在循环体中的代码执行之后，才会测试出口条件。 换句话说，在对条件表达式求值之前，循环体内的代码至少会被执行一次。

```javascript
do {
 statement
} while (expression); 
```



### while语句

while 语句属于前测试循环语句，也就是说，在循环体内的代码被执行之前，就会对出口条件求值。 因此，循环体内的代码有可能永远不会被执行。

```javascript
while(expression) 
    statement 
```



### for语句

for 语句也是一种前测试循环语句，但它具有在执行循环之前初始化变量和定义循环后要执行的代 码的能力。

```javascript
for (initialization; expression; post-loop-expression) 
    statement
```

由于 ECMAScript 中不存在块级作用 域，因此在循环内部定义的变量也可以在外部访问到。也就是说for语句中的初始化的位置不影响结果。

​	

### for-in语句

for-in 语句是一种精准的迭代语句，可以用来枚举对象的属性

```javascript
for (property in expression) 
    statement 
```

建议在使用 for-in 循环之前，先检测确认该对象的值不是 null 或 undefined。



### label语句

使用 label 语句可以在代码中添加标签，以便将来使用。

```javascript
//label: statement
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
```

加上标签主要用于跳出内层循环，进行外层操作



### break和continue语句

break 语句会立即退出循环， 强制继续执行循环后面的语句。

continue 语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行。

与标签结合使用时可跳出多层循环



### with语句

with 语句的作用是将代码的作用域设置到一个特定的对象中。

```javascript
//with (expression) statement; 
//var qs = location.search.substring(1);
//var hostName = location.hostname;
//var url = location.href; 
with(location){
 var qs = search.substring(1);
 var hostName = hostname;
 var url = href;
}
```

严格模式下不允许使用 with 语句，否则将视为语法错误。



### switch语句

```javascript
switch (expression) {
 case value: statement
 	break;
 case value: statement
 	break;
 case value: statement
 	break;
 case value: statement
 	break; 
 default: statement
} 
```

首先，可以在 switch 语句中使用任何数据类型（在很多其他语言中只能使用数值），无论是字符串，还是对象都没有 问题。其次，每个 case 的值不一定是常量，可以是变量，甚至是表达式。