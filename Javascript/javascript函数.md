## 函数

严格模式对函数有一些限制： 

1. 不能把函数命名为 eval 或 arguments； 
2. 不能把参数命名为 eval 或 arguments； 
3. 不能出现两个命名参数同名的情况。 

如果发生以上情况，就会导致语法错误，代码无法执行。



定义函数的 方式有两种：一种是函数声明，另一种就是函数表达式。

### 函数声明

```javascript
function functionName(arg0, arg1, arg2) {
 //函数体
} 
```

函数声明的一个重要特征就是函数声明提升（function declaration hoisting），意思是在执行 代码之前会先读取函数声明。这就意味着可以把函数声明放在调用它的语句后面。

```javascript
sayHi();
function sayHi(){
 alert("Hi!");
} 
```



### 函数表达式

```javascript
var functionName = function(arg0, arg1, arg2){
 //函数体
}; 
```

这种形式看起来好像是常规的变量赋值语句，即创建一个函数并将它赋值给变量 functionName。 这种情况下创建的函数叫做匿名函数（anonymous function），因为 function 关键字后面没有标识符。 （匿名函数有时候也叫拉姆达函数。）匿名函数的 name 属性是空字符串。

函数表达式与其他表达式一样，在使用前必须先赋值。也就是说函数表达式不存在函数提升

```javascript
sayHi(); //错误：函数还不存在
var sayHi = function(){
 alert("Hi!");
}; 
```

因为有函数提升这个特性，函数声明不能嵌套在部分语句块中，但是函数表达式却可以

```javascript
//不要这样做！
if(condition){
 function sayHi(){
 alert("Hi!");
 }
} else {
 function sayHi(){
 alert("Yo!");
 }
} 

//可以这样做
var sayHi;
if(condition){
 sayHi = function(){
 alert("Hi!");
 };
} else {
 sayHi = function(){
 alert("Yo!");
 };
} 
```

在浏览器中，第一种情况中函数声明会被覆盖，而第二种情况则是会赋值到sayHi函数中。

### 匿名函数与函数表达式

####匿名函数

```javascript
(function(){
    //do something
})()
```

以上代码定义并调用了匿名函数，看上去是以一种全新的模式，但其实它只是一种函数表达式，

我们可以将它写成另一种形式

```javascript
var someFunction=function(){
	//do something
}
someFunction()
```

以上代码其实就是将第一段代码进行拆分

当我们将匿名函数写成这样时，它显示出错

```javascript
function(){
    //do something
}()
```

以为在function被当作一个函数声明来进行解析，函数声明不能带有()，而当加上一个一个括号后，他就会被认为时函数表达式，这样的话就能带上括号直接进行调用。这种匿名函数其实就是函数表达式的合并



### 递归

递归函数是在一个函数通过名字调用自身的情况下构成的

```javascript
function factorial(num){
 	if (num <= 1){
 		return 1;
 	} else {
 		return num * factorial(num-1);
 	}
} 
```

这个函数表面看来没什么问题，但下面的代码却可能导致它出错

```javascript
var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4)); //出错！
```

在这个例子中，调用anotherFactorial()时会递归调用factorial()，但是factorial()函数已经被置为null，递归调用时不会产生作用，这时我们可以通过argument.callee进行解决这个问题

```javascript
function factorial(num){
 	if (num <= 1){
 		return 1;
 	} else {
 		return num * arguments.callee(num-1);
 	}
} 
```

通过使用 arguments.callee 代替函数名，可以确保无论怎样调用函数都不会 出问题。因此，在编写递归函数时，使用 arguments.callee 总比使用函数名更保险。 

但在严格模式下，不能通过脚本访问 arguments.callee，访问这个属性会导致错误。不过，可 以使用命名函数表达式来达成相同的结果

```javascript
var factorial = (function f(num){
 	if (num <= 1){
 		return 1;
 	} else {
 		return num * f(num-1);
 	}
}); 
```



### 闭包

闭包是指有权访问另一个 函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数。

```javascript
function createComparisonFunction(propertyName) {

 return function(object1, object2){
        var value1 = object1[propertyName];
        var value2 = object2[propertyName];

        if (value1 < value2){
            return -1;
        } else if (value1 > value2){
            return 1;
        } else {
            return 0;
        }
    };
} 
```

在例子中内部函数的代码访问了外部函数中的变量propertyName，之所以能够访问这个变量，是因为内部函数的作用域链中包含 createComparisonFunction()的作用域。

当某个函数被调用时，会创建一个执行环境（execution context）及相应的作用域链。 然后，使用 arguments 和其他命名参数的值来初始化函数的活动对象（activation object）。但在作用域 链中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位，……直至作为作用域链终点的全局执行环境

在函数执行过程中，为读取和写入变量的值，就需要在作用域链中查找变量。

```javascript
function compare(value1, value2){
     if (value1 < value2){
     	return -1;
     } else if (value1 > value2){
     	return 1;
     } else {
     	return 0;
     }
  }
var result = compare(5, 10); 
```

当调用 compare()时，会 创建一个包含 arguments、value1 和 value2 的活动对象。全局执行环境的变量对象（包含 result 和 compare）在 compare()执行环境的作用域链中则处于第二位。



![image-20200415164116813](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200415164116813.png)

全局环境的变量对象始终存在，而像 compare()函数这样的局部环境的变量对象，则只在函数执行的过程中存在。在创建 compare()函数 时，会创建一个预先包含全局变量对象的作用域链，这个作用域链被保存在内部的[[Scope]]属性中。 当调用 compare()函数时，会为函数创建一个执行环境，然后通过复制函数的[[Scope]]属性中的对 象构建起执行环境的作用域链。此后，又有一个活动对象（在此作为变量对象使用）被创建并被推入执 行环境作用域链的前端。对于这个例子中 compare()函数的执行环境而言，其作用域链中包含两个变 量对象：本地活动对象和全局变量对象。显然，作用域链本质上是一个指向变量对象的指针列表，它只引用但不实际包含变量对象

无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来讲， 当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）。 但是，闭包的情况又有所不同。

在另一个函数内部定义的函数会将包含函数（即外部函数）的活动对象添加到它的作用域链中。因 此，在 createComparisonFunction()函数内部定义的匿名函数的作用域链中，实际上将会包含外部 函数 createComparisonFunction()的活动对象。

```javascript
var compare = createComparisonFunction("name");
var result = compare({ name: "Nicholas" }, { name: "Greg" });
```



在匿名函数从createComparisonFunction()中被返回后，它的作用域链被初始化为包含 createComparisonFunction()函数的活动对象和全局变量对象。这样，匿名函数就可以访问在 createComparisonFunction()中定义的所有变量。更为重要的是，createComparisonFunction() 函数在执行完毕后，其活动对象也不会被销毁，因为匿名函数的作用域链仍然在引用这个活动对象。换句话说，当 createComparisonFunction()函数返回后，其执行环境的作用域链会被销毁，但它的活动对象仍然会留在内存中；直到匿名函数被销毁后，createComparisonFunction()的活动对象才会被销毁

```javascript
//解除对匿名函数的引用（以便释放内存）
compareNames = null; 
```

![image-20200415171007884](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200415171007884.png)

总而言之，闭包能够携带包含它函数的作用域，即使外部函数结束，但它仍会调用外部函数，因此会占用更多的内存。



#### 闭包与变量

作用域链的机制会导致一个特性，就是闭包只能缺德外部函数中的变量最后一个值。



在代码执行过程中，执行上下文栈的行为如下

```
/*伪代码*/
// 代码执行时最先进入全局环境，全局上下文被创建并入栈
ECStack.push(globalContext);
// fn1 被调用，fn1 函数上下文被创建并入栈
ECStack.push(<fn1> functionContext);
// fn1 执行完毕，fn1 函数上下文出栈
ECStack.pop();
// fn2 被调用，fn2 函数上下文被创建并入栈
ECStack.push(<fn2> functionContext);
// fn2 执行完毕，fn2 函数上下文出栈
ECStack.pop();
// 代码执行完毕，全局上下文出栈
ECStack.pop();
```

在作用域链机制中，fn1在执行完之后就会被垃圾回收机制回收，但fn2执行中会再次调用fn1的变量对象，导致fn1中的变量对象无法被回收。

**函数作用域是在函数被定义（声明）的时候确定的**。每一个函数都会包含一个 [[scope]] 内部属性，在函数被定义的时候，该函数的 [[scope]] 属性会保存其上层上下文的变量对象，形成包含上层上下文变量对象的层级链。fn2被定义时，就包括fn1的上下文和全局上下文。所以fn2被调用时，会将上下文创建并入栈，此时会生成变量对象并将该变量对象添加进作用域链的顶端，并且将 [[scope]] 添加进作用域链

#####具体例子

```javascript
var arr = [];
for (var i = 0; i < 3; i++) {
    arr[i] = function () {
        console.log(i);
    };
}

arr[0]();//3
arr[1]();//3
arr[2]();//3
```

根据作用域链，arr[0] 函数会现在自身的变量对象中寻找 i ，如果找不到，会到全局上下文变量对象中寻找 i，所以最终输出 3 。

#####解决方式

通过匿名函数解决

```javascript
var arr = [];
for (var i = 0; i < 3; i++) {
    arr[i] = (function (i) {
        return function(){
            console.log(i);
        }
    })(i);
}

arr[0]();  //  0
arr[1]();  //  1
arr[2]();  //  2
```

通过匿名函数，可以将最内层的函数紧跟匿名函数的作用域，而不是跟着最外层函数，这样就能解决问题。

[资料来源]:https://juejin.im/post/5b3ae7b8f265da62cb1db9e0



### this对象

在闭包中使用 this 对象也可能会导致一些问题。我们知道，this 对象是在运行时基于函数的执 行环境绑定的：在全局函数中，this 等于 window，而当函数被作为某个对象的方法调用时，this 等 于那个对象。不过，匿名函数的执行环境具有全局性，因此其 this 对象通常指向 window。

```javascript
var name = "The Window";
var object = {
	 name : "My Object",
	 getNameFunc : function(){
		 return function(){
			 return this.name;
		 };
	 }
};
alert(object.getNameFunc()()); //"The Window"（在非严格模式下）
```

在这个例子中匿名函数返回的是"This Window"而不是"My Object"，因为在函数调用时都会自动取得两个变量，"this"和"arguments"。内部函数在搜索这两个变量时，一旦搜索到就会停止，这导致匿名函数作用域链上一旦搜索到全局环境中的"this"和"arguments"就会停止（因为匿名函数执行环境具有全局性）。

上面的例子可以写成另一种形式

```javascript
var name = "The Window";
function getThisName(){
    return this.name
}
var object = {
    name: "My Object",
    getNameFunc:getThisName 
};
console.log(object.getNameFunc()); // 输出: My Object
var getThisNameFunc = object.getNameFunc; //只是赋值，不是调用，因此不会改变this
console.log(getThisNameFunc()); // 输出: The Window
```

因为匿名函数执行环境具有全局性，我们可以将其写在外部，这样更方便理解。

####调用函数中this的方法

```javascript
var name = "The Window";
var object = {
	 name : "My Object",
	 getNameFunc : function(){
          var that=this;
		 return function(){
			 return that.name;
		 };
	 }
};
alert(object.getNameFunc()()); //"My Object"
```

在调用函数前，将this进行更改，就可以直接调用到函数中的this。

####改变this的情况

```javascript
var name = "The Window";
var object = {
     name : "My Object",
     getName: function(){
     	return this.name;
     }
}; 
object.getName(); //"My Object"
(object.getName)(); //"My Object"
(object.getName = object.getName)(); //"The Window"，在非严格模式下
```

第一和第二中方法只是调用了Object.getName()，因为不存在匿名函数，所以this不是The Window。而在第三中方法中，可以理解为创建了一个匿名函数

```javascript
(object.getName:function(){
	return object.getName	
})()
```

所以它的this为The Window。



###内存泄漏

如前文所说，闭包需要手动释放内存。而对于一些HTML元素来说，这是无法做到的。所以可能导致内存泄漏。就如下面这个例子

```javascript
function assignHandler(){
 	var element = document.getElementById("someElement");
	element.onclick = function(){
		alert(element.id);
 	};
} 
```

element元素无法被消除，内存也无法被回收。我们可以通过改写这段代码来进行内存回收

```javascript
function assignHandler(){
 	var element = document.getElementById("someElement");
 	var id = element.id;

 	element.onclick = function(){
 		alert(id);
 	};

 	element = null;
} 
```

将element元素中所需的变量用另一个变量进行保存，这样就能避免element一直被调用，无法释放内存的问题。

### 块级作用域

javascript中不存在块级作用域，也就是说不会像java，c++等语言中会销毁块级作用域的变量。

```javascript
function outputNumbers(count){
 	for (var i=0; i < count; i++){
 		alert(i);
 	}
 	alert(i); //计数
} 
```

在这个函数中，for循环在Java，c++等语言中有着语句块，变量i 只会在语句块中有定义，一旦结束循环，i就会进行销毁，而在JavaScript中，i是定义在函数中，而不是语句块中，所以可以在函数各个位置访问它。

而用作块级作用域（私有作用域）的匿名函数能够避免这个问题

```javascript
function outputNumbers(count){
    (function () {
     	for (var i=0; i < count; i++){
     		alert(i);
    	}
    })();

 	alert(i); //导致一个错误！
}
```



###私有变量

严格来讲，JavaScript 中没有私有成员的概念；所有对象属性都是公有的。不过，倒是有一个私有 变量的概念。任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。 私有变量包括函数的参数、局部变量和在函数内部定义的其他函数。

如果在这个函数内部创建一个闭包，那么闭包通过自己的作用域链也可以访 问这些变量。而利用这一点，就可以创建用于访问私有变量的公有方法

我们把有权访问私有变量和私有函数的公有方法称为特权方法（privileged method）。有两种在对象 上创建特权方法的方式。第一种是在构造函数中定义特权方法：

```javascript
function MyObject(){
     //私有变量和私有函数
     var privateVariable = 10;
     function privateFunction(){
     	return false;
     }
     //特权方法
     this.publicMethod = function (){
     	privateVariable++;
     	return privateFunction();
     };
} 
```

这个模式在构造函数内部定义了所有私有变量和函数。然后，又继续创建了能够访问这些私有成员 的特权方法。能够在构造函数中定义特权方法，是因为特权方法作为闭包有权访问在构造函数中定义的 所有变量和函数。

利用私有和特权成员，可以隐藏那些不应该被直接修改的数据。

```javascript
function Person(name){
 	this.getName = function(){
 		return name;
 	};
 	this.setName = function (value) {
 		name = value;
 	};
}
var person = new Person("Nicholas");
alert(person.getName()); //"Nicholas"
person.setName("Greg");
alert(person.getName()); //"Greg" 
```

这个例子中，只能通过给定的getName和setName来进行查询和修改name属性，避免了Name属性被直接访问。

#### 静态私有变量

通过在私有作用域中定义私有变量或函数，同样也可以创建特权方法。

```javascript
(function(){
 //私有变量和私有函数
 var privateVariable = 10;
 function privateFunction(){
 	return false;
 }
 //构造函数
 MyObject = function(){
 };
 //公有/特权方法
 MyObject.prototype.publicMethod = function(){
 	privateVariable++;
 	return privateFunction();
 };
})();
```

这个模式创建了一个私有作用域，并在其中封装了一个构造函数及相应的方法。在私有作用域中， 首先定义了私有变量和私有函数，然后又定义了构造函数及其公有方法。

这个模式与在构造函数中定义特权方法的主要区别，就在于私有变量和函数是由实例共享的。由于 特权方法是在原型上定义的，因此所有实例都使用同一个函数。而这个特权方法，作为一个闭包，总是 保存着对包含作用域的引用。

```javascript
(function(){

 var name = "";

 Person = function(value){
 	name = value;
 };

 Person.prototype.getName = function(){
 	return name;
 };

 Person.prototype.setName = function (value){ 
 	name = value;
 };
})();
var person1 = new Person("Nicholas");
alert(person1.getName()); //"Nicholas"
person1.setName("Greg");
alert(person1.getName()); //"Greg"
var person2 = new Person("Michael");
alert(person1.getName()); //"Michael"
alert(person2.getName()); //"Michael" 
```

#### 模块模式

道格拉斯所说的模块模式（module pattern）则是为单例创建私有变量和特权方法。所谓单例（singleton），指的就是只有一个实例的对象。

模块模式通过为单例添加私有变量和特权方法能够使其得到增强，其语法形式如下：

```javascript
var singleton = function(){

     //私有变量和私有函数
     var privateVariable = 10;

     function privateFunction(){
        return false;
     } 
     //特权/公有方法和属性
     return {
     	publicProperty: true,
     	publicMethod : function(){
     		privateVariable++;
     		return privateFunction();
     	}
     };
}(); 
```

这个模块模式使用了一个返回对象的匿名函数。在这个匿名函数内部，首先定义了私有变量和函数。 然后，将一个对象字面量作为函数的值返回。返回的对象字面量中只包含可以公开的属性和方法。

从本质上来讲，这个 对象字面量定义的是单例的公共接口。这种模式在需要对单例进行某些初始化，同时又需要维护其私有 变量时是非常有用的。

#### 增强的模块模式

这种增强的模块模式适合那 些单例必须是某种类型的实例，同时还必须添加某些属性和（或）方法对其加以增强的情况。

```javascript
var singleton = function(){
     //私有变量和私有函数
     var privateVariable = 10;
	    function privateFunction(){
    	return false;
     }
     //创建对象
     var object = new CustomType();
     //添加特权/公有属性和方法
     object.publicProperty = true;
     object.publicMethod = function(){
     	privateVariable++;
     	return privateFunction();
     };
     //返回这个对象
     return object;
}(); 

```



### 参数

ECMAScript 函数的参数与大多数其他语言中函数的参数有所不同。ECMAScript 函数不介意传递进 来多少个参数，也不在乎传进来参数是什么数据类型。也就是说，即便你定义的函数只接收两个参数， 在调用这个函数时也未必一定要传递两个参数。

ECMAScript 中的参数在内部是用一个数组来表示的。函数接收 到的始终都是这个数组，而不关心数组中包含哪些参数（如果有参数的话）。如果这个数组中不包含任何元素，无所谓；如果包含多个元素，也没有问题。

实际上，在函数体内可以通过 arguments 对象来 访问这个参数数组，从而获取传递给函数的每一个参数。arguments 对象只是与数组类似（它并不是 Array 的实例），因为可以使用方括号语法访 问它的每一个元素（即第一个元素是 arguments[0]，第二个元素是 argumetns[1]，以此类推），使 用 length 属性来确定传递进来多少个参数。

ECMAScript 函数的一个重要特点：命名的参数只提供便利，但不是必需的。在命名参数方面，其他语言可能需要事先创建一个函数签名，而将来的调用必须与该签名一致。但在ECMAScript中，没有这些条条框框，解析器不会验证命名参数。

```javascript
function howManyArgs() {
 alert(arguments.length);
}
howManyArgs("string", 45); //2
howManyArgs(); //0
howManyArgs(12); //1 
```



### 没有重载

ECMAScirpt 函数没有签名，因为其参数是由包含零或多个值的数组来表示的。而没有函数签名，真正的重载是不可能做到的。 如果在 ECMAScript 中定义了两个名字相同的函数，则该名字只属于后定义的函数

```javascript
function addSomeNumber(num){
 return num + 100;
}
function addSomeNumber(num) {
 return num + 200;
}
var result = addSomeNumber(100); //300 
```

