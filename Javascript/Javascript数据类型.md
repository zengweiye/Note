## javascript数据类型

---

ECMAScript中有5中简单数据类型：Undefined,Null,Boolean,String,Number，以及1种复杂数据类型：Object



### Typeof操作符

对一个值进行typeof操作时，可能会产生以下结果

- Undefined ——未定义
- Boolean——布尔值
- String——字符串
- Number——数值
- Object——对象或者Null
- Function——函数

值得注意的是：当调用typeof Null 时，结果时Object，也就是说typeof 的结果不存在基础数据类型的Null。因为Null被认为是空的对象引用。



### Undefined

当一个变量只声明但没有进行初始化时，它的值就为undefined

```javascript
var message;
alert(message==undefined);//true
```

如果一个变量没有声明直接调用时，则会产生错误，但通过调用typeof时，则会输出Undefined

```javascript
var message;
alert(message);//undefined
alert(other);//产生错误
alert(typeof(message));//undefined
alert(typeof(other));//undefined
```



### Null

从逻辑角度来看，null 值表示一个空对象指针，而这也正是使用 typeof 操作符检测 null 值时会返回"object"的原因

```javascript
var message=null;
alert(typeof(message));//object
```

如果变量将要在以后使用，建议将变量初始化为null，以后只需要检测对象是否为null即刻知道变量是否使用。

实际上，Undefined是派生自Null的

```javascript
alert(null==undefined);//true
```



### Boolean

Boolean有两个值，分别是true和false。需要注意的是true和false对大小写敏感。

要将一个值转换成Boolean可以通过Boolean()。返回的Boolean取决于转换值的数据类型以及实际值

| 数据类型  | true           | false     |
| :-------- | -------------- | --------- |
| Boolean   | true           | false     |
| String    | 任意非空字符串 | 空字符串  |
| Number    | 任意非0的数    | 0或NaN    |
| Object    | 任意对象       | null      |
| Undefined | 不适用         | undefined |



### Number

####整型

不同进制的表达方式：

```javascript
var intNum = 55; // 整数
var octalNum1 = 070; // 八进制的 56
var octalNum2 = 079; // 无效的八进制数值——解析为 79
var octalNum3 = 08; // 无效的八进制数值——解析为 8
var hexNum1 = 0xA; // 十六进制的 10
var hexNum2 = 0x1f; // 十六进制的 31
```

八进制在超出范围时会自动解析为十进制



####浮点型

浮点数必须包含一个小数点以及后面至少有一个数字，如果小数点后没有数字，将会自动解析成整型。极大或者极小的数字可用科学计数法

```javascript
var floatNum1 = 1.1;
var floatNum2 = 0.1;
var floatNum3 = .1; // 有效，但不推荐
var floatNum4 = 1.; // 小数点后面没有数字——解析为 1
var floatNum5 = 10.0; // 整数——解析为 10
var floatNum6 = 3.125e7; // 等于 31250000
```

需要注意的是，浮点数的最高精密度为17位，但在进行算术计算时其精确度远远不如整数。因为小数在禁止转换和进行对阶运算时，会产生精度损失，导致结果产生偏差。



#### NaN

NaN，即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数 未返回数值的情况（这样就不会抛出错误了）

特点：

1. 任何涉及NaN的操作都会返回NaN
2. NaN不与其他数值相等，包括其本身

```javascript
alert(NaN == NaN); //false 
```

isNaN函数，则是用来确定参数是否NaN

```javascript
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false（10 是一个数值）
alert(isNaN("10")); //false（可以被转换成数值 10）
alert(isNaN("blue")); //true（不能转换成数值）
alert(isNaN(true)); //false（可以被转换成数值 1）
```



#### 数值转换

数值转换函数有三种：Number()、parseInt()和 parseFloat()。

#####Number()函数转换规则：

- 如果是 Boolean 值，true 和 false 将分别被转换为 1 和 0。
- 如果是数字值，只是简单的传入和返回。 
- 如果是 null 值，返回 0。
- 如果是 undefined，返回 NaN。
- 如果是字符串，遵循下列规则
  - 如果符合十进制或十六进制规则，直接根据进制直接转换
  - 如果是空字符串，则为0
  - 如果不符合以上字符，则为NaN

```javascript
var num1 = Number("Hello world!"); //NaN
var num2 = Number(""); //0
var num3 = Number("000011"); //11
var num4 = Number(true); //1 
```

#####parseInt()函数转换规则：

- 它会忽略字 符串前面的空格，直至找到第一个非空格字符
- 如果第一个字符不是数字字符或者负号，parseInt() 就会返回 NaN，即转换空字符串会返回 NaN
- 如 果第一个字符是数字字符，parseInt()会继续解析第二个字符，直到解析完所有后续字符或者遇到了 一个非数字字符。
- 如果字符串以"0x"开头且后跟数字字符，就会将其当作一 个十六进制整数；如果字符串以"0"开头且后跟数字字符，则会将其当作一个八进制数来解析。（八进制在ECMAScript5中已经取消）
- 可以为这个函数提供第二个参数：转换时使用的基数（即多少进制）

```javascript
var num1 = parseInt("1234blue"); // 1234
var num2 = parseInt(""); // NaN
var num3 = parseInt("0xA"); // 10（十六进制数）
var num4 = parseInt(22.5); // 22
var num6 = parseInt("70"); // 70（十进制数）
var num7 = parseInt("0xf"); // 15（十六进制数）
var num8 = parseInt("0xAF", 16); //175 
var num9 = parseInt("AF", 16); //175
var num0 = parseInt("AF"); //NaN 
```

#####parseFloat()函数转换规则：

- 字符串中的第一个小数点是有效的，而第二个小数点就是无效的了。
- 始终都会忽略前导的零。
- 其他规则与parseInt规则一样

```javascript
var num1 = parseFloat("1234blue"); //1234 （整数）
var num2 = parseFloat("0xA"); //0
var num3 = parseFloat("22.5"); //22.5
var num4 = parseFloat("22.34.5"); //22.34
var num5 = parseFloat("0908.5"); //908.5
var num6 = parseFloat("3.125e7"); //31250000 
```

### String

与 PHP 中的双引号和单引号会影响对字符串的解释方式不同，ECMAScript 中的这两种语法形式没 有什么区别。用双引号表示的字符串和用单引号表示的字符串完全相同。

####字符串特点

ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变 某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量

####转换成字符串

数值、布尔值、对象和字符串值（没错，每个字符串也都有一个 toString()方法，该方法返回字 符串的一个副本）都有 toString()方法。但 null 和 undefined 值没有这个方法。

而通过传递基数，toString()可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格 式表示的字符串值。

```javascript
var num = 10;
alert(num.toString()); // "10"
alert(num.toString(2)); // "1010"
alert(num.toString(8)); // "12"
alert(num.toString(10)); // "10"
alert(num.toString(16)); // "a" 
```



### Object

ECMAScript 中的对象其实就是一组数据和功能的集合。对象可以通过执行 new 操作符后跟要创建 的对象类型的名称来创建。而创建 Object 类型的实例并为其添加属性和（或）方法，就可以创建自定 义对象，如下所示： 

```javascript
var o = new Object(); 
```

 ECMAScript 中， （就像 Java 中的 java.lang.Object 对象一样）Object 类型是所有它的实例的基础。换句话说， Object 类型所具有的任何属性和方法也同样存在于更具体的对象中。



### 操作符

#### 一元操作符

1. 递增和递减：++/-- 直接将变量增加或减少1
2. 一元加和减运算符：先进行像Number()一样的解析，再进行转换正负数



#### 位操作符

位操作符用于在最基本的层次上，即按内存中表示数值的位来操作数值。ECMAScript 中的所有数 值都以 IEEE-754 64 位格式存储，但位操作符并不直接操作 64 位的值。而是先将 64 位的值转换成 32 位 的整数，然后执行操作，最后再将结果转换回 64 位。

对于有符号的整数，32 位中的前 31 位用于表示整数的值。第 32 位用于表示数值的符号：0 表示正 数，1 表示负数。这个表示符号的位叫做符号位，符号位的值决定了其他位数值的格式。

负数同样以二进制码存储，但使用的格式是进制补码（取反加一）



**主要操作符**

1. 按位非~：返回数值的反码
2. 按位与&：返回两个数的AND操作结果
3. 按位或 |：返回两个数的OR操作结果
4. 按位异或^：返回两个数XOR操作结果
5. 左移<<：以 0 来填充移动产生的空位。注意：左移不会影响操作数的符号位
6. 有符号右移>>：以符号位填充移动产生的空位。
7. 无符号右移>>>：以0填充移动产生的空位



#### 布尔操作符

1. 逻辑非 ! ：如果操作数是对象，非空字符串，非零的数则为false，其他为true
2. 逻辑与&&：
   1. 如果第一个操作数是对象，则返回第二个操作数；
   2.  如果第二个操作数是对象，则只有在第一个操作数的求值结果为 true 的情况下才会返回该对象； 
   3. 如果两个操作数都是对象，则返回第二个操作数； 
   4. 如果有一个操作数是 null，则返回 null；
   5. 如果有一个操作数是 NaN，则返回 NaN； 
   6. 如果有一个操作数是 undefined，则返回 undefined。
   7. 逻辑与操作属于短路操作，即如果第一个操作数能够决定结果，那么就不会再对第二个操作数求值
3. 逻辑或 || ：
   1. 如果第一个操作数是对象，则返回第一个操作数； 
   2. 如果第一个操作数的求值结果为 false，则返回第二个操作数； 
   3. 如果两个操作数都是对象，则返回第一个操作数； 
   4. 如果两个操作数都是 null，则返回 null； 
   5. 如果两个操作数都是 NaN，则返回 NaN； 
   6. 如果两个操作数都是 undefined，则返回 undefined。
   7. 与逻辑与操作符相似，逻辑或操作符也是短路操作符。也就是说，如果第一个操作数的求值结果为 true，就不会对第二个操作数求值了。