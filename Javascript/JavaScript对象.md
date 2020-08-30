##对象

### 理解对象

#### 属性类型

#####数据属性

1. [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。默认为true
2. [[Enumerable]]：表示能否通过 for-in 循环返回属性。默认为true
3. [[Writable]]：表示能否修改属性的值。默认为true
4. [[Value]]：包含这个属性的数据值。默认为undefined

```javascript
var person = {};
Object.defineProperty(person, "name", {
     writable: false,
     value: "Nicholas"
});
alert(person.name); //"Nicholas"
person.name = "Greg";
alert(person.name); //"Nicholas" 
```



##### 访问器属性

**访问器属性不能直接定义，必须使用 Object.defineProperty()来定义。**

1. [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特 性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为 true。
2. [[Enumerable]]：表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这 个特性的默认值为 true。
3. [[Get]]：在读取属性时调用的函数。默认值为 undefined。
4. [[Set]]：在写入属性时调用的函数。默认值为 undefined。

```javascript
var book = {
     _year: 2004,
     edition: 1
};
Object.defineProperty(book, "year", {
     get: function(){
   		 return this._year;
	 },
 	set: function(newValue){
         if (newValue > 2004) {
         this._year = newValue;
         this.edition += newValue - 2004;
         }
 	}
});
book.year = 2005;
alert(book.edition); //2 
```

_year 前面 的下划线是一种常用的记号，用于表示只能通过对象方法访问的属性。



#### 定义多个属性

通过Object.defineProperties()方法可以一次定义多个属性。

这个方法接收两个对象参数：第一 个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应。

```javascript
var book = {};
Object.defineProperties(book, {
     _year: {
         value: 2004
     },

     edition: {
         value: 1
     },
     year: {
         get: function(){
              return this._year;
         },
        set: function(newValue){
                 if (newValue > 2004) {
                     this._year = newValue;
                     this.edition += newValue - 2004;
                 }
             }
     }
}); 
```



#### 读取属性特性

Object.getOwnPropertyDescriptor()方法，可以取得给定属性的描述符。这个方法接收两个参数：属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable、enumerable、get 和 set；如果是数据属性，这个对象的属性有 configurable、enumerable、writable 和 value。

```javascript
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value); //2004
alert(descriptor.configurable); //false 
alert(typeof descriptor.get); //"undefined"
var descriptor = Object.getOwnPropertyDescriptor(book, "year");
alert(descriptor.value); //undefined
alert(descriptor.enumerable); //false
alert(typeof descriptor.get); //"function"
```



### 创建对象

#### 工厂模式

通过用函数来封装以特定接口创建对象的细节

```javascript
function createPerson(name, age, job){
     var o = new Object();
         o.name = name;
         o.age = age;
         o.job = job;
         o.sayName = function(){
         alert(this.name);
     };
 return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor"); 
```

通过函数来创建对象，封装起来更加方便创建。



#### 构造函数模式

ECMAScript 中的构造函数可用来创建特定类型的对象。可以通过创建自定义的构造函数，从而定义自定义对象类型的属性和方法。

```javascript
function Person(name, age, job){
     this.name = name;
     this.age = age;
     this.job = job;
     this.sayName = function(){
        alert(this.name);
     };
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```

按照惯例，构造函数始终都应该以一个 大写字母开头，而非构造函数则应该以一个小写字母开头。

要创建 Person 的新实例，必须使用 new 操作符。

**调用构建函数的步骤**

1. 创建一个新对象； 
2. 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
3. 执行构造函数中的代码（为这个新对象添加属性）；
4. 返回新对象。

##### 将构造函数当作函数

构造函数与其他函数的唯一区别，就在于调用它们的方式不同。不过，构造函数毕竟也是函数，不 存在定义构造函数的特殊语法。任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数；而 任何函数，如果不通过 new 操作符来调用，那它跟普通函数也不会有什么两样。

````javascript
// 当作构造函数使用
var person = new Person("Nicholas", 29, "Software Engineer");
person.sayName(); //"Nicholas"
// 作为普通函数调用
Person("Greg", 27, "Doctor"); // 添加到 window
window.sayName(); //"Greg"
// 在另一个对象的作用域中调用
var o = new Object();
Person.call(o, "Kristen", 25, "Nurse");
o.sayName(); //"Kristen" 
````

##### 构造函数的问题

构造函数在实例对象时，会把每个实例重新创建一次，这在构造Function实例时，会导致不同的作用域链和标识符解析，但创造Function新实例的机制仍然时一样的。所以不同实例上的同名函数是不一样的。

```javascript
alert(person1.sayName == person2.sayName); //false 
```

但因为有this的存在，不需要一开始就将函数绑定在特定对象上，只需要使用时调用外部共用函数就好

```javascript
function Person(name, age, job){
     this.name = name;
     this.age = age;
     this.job = job;
     this.sayName = sayName;
}
function sayName(){
	 alert(this.name);
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor"); 
```



##### 原型模式

我们创建的每个函数都有一个 prototype（原型）属性，这个属性是一个指针，指向一个对象， 而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。

使用原型对象的好处是可以 让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中。

```javascript
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	console.log(this.name);
};
var person1 = new Person();
person1.sayName(); //"Nicholas"
var person2 = new Person();
person2.sayName(); //"Nicholas"
console.log(person1.sayName == person2.sayName); //true 
```

###### 理解原型对象

只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype 属性，这个属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个 constructor （构造函数）属性，这个属性包含一个指向 prototype 属性所在函数的指针。

创建了自定义的构造函数之后，其原型对象默认只会取得 constructor 属性；至于其他方法，则都是从 Object 继承而来的。

![image-20200427200332970](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200427200332970.png)

person1和person2都包含一个prototype属性，并指向了Person.prototype；也就是说实例以构造函数之间没有直接的关系。而之所以实例能够调用到方法，只是通过查找对象属性来实现的。

每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例本身开始。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到， 则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。如果在原型对象中找到了这个属性，则返回该属性的值。

按照这种特性，原型就相当于对象一个基础值，在没有进行具体实例化时，始终保持这一个不变的值。而在实例化后，就会随着实例而变化。

```javascript
person1.name = "Greg";
alert(person1.name); //"Greg"——来自实例
alert(person2.name); //"Nicholas"——来自原型
delete person1.name;
alert(person1.name); //"Nicholas"——来自原型
```

当我们实例化对象时，原型的值并非直接改变，而是被**屏蔽**，也可以说是因为有了实例的值后，在搜寻属性时不会深入到原型，直接在实例就能找到对象属性。而一旦我们将实例的属性删除后，原本在实例上的属性消失，在搜寻时就会再次深入到原型。

![hasOwnProperty](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200427202657255.png)



###### in操作符与hasOwnProperty()方法

in 操作符会在通过对象能够访问给定属性时返回 true，无论该属性存在于实例中还是原型中。

hasOwnProperty()方法能判断属性是否在实例中。

```javascript
person1.name = "Greg";
alert(person1.name); //"Greg" ——来自实例
alert(person1.hasOwnProperty("name")); //true
alert("name" in person1); //true
alert(person2.name); //"Nicholas" ——来自原型
alert(person2.hasOwnProperty("name")); //false
alert("name" in person2); //true
delete person1.name;
alert(person1.name); //"Nicholas" ——来自原型
alert(person1.hasOwnProperty("name")); //false
alert("name" in person1); //true 
```



