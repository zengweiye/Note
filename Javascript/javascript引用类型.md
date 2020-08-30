## 引用类型

### Object类型

####创建方法

第一种：**new建立**

```javascript
var person = new Object();
person.name = "Nicholas";
person.age = 29; 
```

第二种：**对象字面表示法**

```javascript
var person = {
	name : "Nicholas",
	age : 29
}; 
```

也可以用**字符串表示**

```javascript
var person = {
	"name" : "Nicholas",
	"age" : 29,
	5 : true
}; 
```

另外，使用对象字面量语法时，如果留空其花括号，则可以定义只包含默认属性和方法的对象，如下所示：

```javascript
var person = {}; //与 new Object()相同
person.name = "Nicholas";
person.age = 29; 
```

#### 访问方法

#####点访问法与方括号访问法

```javascript
alert(person["name"]); //"Nicholas"
alert(person.name); //"Nicholas" 
```

在功能上没有任何区别，但方括号的有点在于通过变量访问属性。

```javascript
var propertyName = "name";
alert(person[propertyName]); //"Nicholas" 
```

如果属性名中包含会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以使用方括号表示法。

```javascript
person["first name"] = "Nicholas"; 
```

虽然方括号功能更多，但是一般推荐使用点访问法，因为这能更好地表示这是个Object属性。



### Array类型

虽然 ECMAScript 数组与其他语言中的数组都是 数据的有序列表，但与其他语言不同的是，ECMAScript 数组的每一项可以保存任何类型的数据。而且，ECMAScript 数组的大小是可以动态调整的，即可以随着数据的添加自动增长以容 纳新增数据。

#### 创建方法

#####构造函数

```javascript
var colors = new Array(3); // 创建一个包含 3 项的数组
var names = new Array("Greg"); // 创建一个包含 1 项，即字符串"Greg"的数组
//省略new
var colors = Array(3); // 创建一个包含 3 项的数组
var names = Array("Greg"); // 创建一个包含 1 项，即字符串"Greg"的数组
```

#####数组字面表达法

```javascript
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
var names = []; // 创建一个空数组
var values = [1,2,]; // 不要这样！这样会创建一个包含 2 或 3 项的数组
var options = [,,,,,]; // 不要这样！这样会创建一个包含 5 或 6 项的数组
```

#### 访问方法

在读取和设置数组的值时，要使用方括号并提供相应值的基于 0 的数字索引

```javascript
var colors = ["red", "blue", "green"]; // 定义一个字符串数组
alert(colors[0]); // 显示第一项
colors[2] = "black"; // 修改第三项
colors[3] = "brown"; // 新增第四项
```

数组的 length 属性不是只读的，通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项。

```javascript
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
colors.length = 2;
alert(colors[2]); //undefined 
colors.length = 4;
alert(colors[3]); //undefined 
```

由于数组最后一项的索引始终是 length-1，因此下一个新项的位置就是 length。每当在数组末 尾添加一项后，其 length 属性都会自动更新以反应这一变化。当把一个值放在超出当前数组大小的位置上时，数组就会重新计算其长度值，即长度值 等于最后一项的索引加 1。

```javascript
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
colors[99] = "black"; // （在位置 99）添加一种颜色
alert(colors.length); // 100 
```

####检验数组

对于一个网页， 或者一个全局作用域而言，使用 instanceof 操作符

```javascript
if (value instanceof Array){
	 //对数组执行某些操作
} 
```

如果网页中包含多个框架，那实 际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的 Array 构造函数。如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。

Array.isArray()方法目的是最终确定某 个值到底是不是数组，而不管它是在哪个全局执行环境中创建的

```javascript
if (Array.isArray(value)){
 //对数组执行某些操作
} 
```

#### 转换方法

数组的转换方法有3种，toLocaleString()、toString()和 valueOf()方法。

toLocaleString()、toString()转换的结果是带有逗号分隔的字符串，而valueof()转换出来的是和原来一样的数组。

所有对象都有自己的toLocaleString()、toString()和 valueOf()方法。但并不是所有的toLocaleString()、toString()返回的结果都是一样的。

```javascript
var person1 = {
 toLocaleString : function () {
 return "Nikolaos";
 },

 toString : function() {
 return "Nicholas";
 }
};
var person2 = {
 toLocaleString : function () {
 return "Grigorios";
 },

 toString : function() {
 return "Greg";
 }
};
var people = [person1, person2];
alert(people); //Nicholas,Greg
alert(people.toString()); //Nicholas,Greg
alert(people.toLocaleString()); //Nikolaos,Grigorios
```

数组继承的 toLocaleString()、toString()和 valueOf()方法，在默认情况下都会以逗号分隔的字 符串的形式返回数组项。而如果使用 join()方法，则可以使用不同的分隔符来构建这个字符串。join()方 法只接收一个参数，即用作分隔符的字符串，然后返回包含所有数组项的字符串。

```javascript
var colors = ["red", "green", "blue"];
alert(colors.join(",")); //red,green,blue
alert(colors.join("||")); //red||green||blue 
```

如果数组中的某一项的值是 null 或者 undefined，那么该值在 join()、 toLocaleString()、toString()和 valueOf()方法返回的结果中以空字符串表示。

#### 栈方法

push()方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。而 pop()方法则从数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项。

```javascript
var colors = new Array(); // 创建一个数组
var count = colors.push("red", "green"); // 推入两项
alert(count); //2
count = colors.push("black"); // 推入另一项
alert(count); //3
var item = colors.pop(); // 取得最后一项
alert(item); //"black"
alert(colors.length); //2 
```



#### 队列方法

队列在列表的末端添加项，从列表的前端移除项。shift()能够移除数组中的第一个项并返回该项，同时将数组长度减 1。

```javascript
var colors = new Array(); //创建一个数组
var count = colors.push("red", "green"); //推入两项
alert(count); //2
count = colors.push("black"); //推入另一项
alert(count); //3
var item = colors.shift(); //取得第一项
alert(item); //"red"
alert(colors.length); //2 
```

unshift()与 shift()的用途相反： 它能在数组前端添加任意个项并返回新数组的长度。

```javascript
var colors = new Array(); //创建一个数组
var count = colors.unshift("red", "green"); //推入两项
alert(count); //2 
count = colors.unshift("black"); //推入另一项
alert(count); //3
var item = colors.pop(); //取得最后一项
alert(item); //"green"
alert(colors.length); //2 
```



#### 重排序方法

#####reverse()

reverse()会反转数组项的顺序。

```javascript
var values = [1, 2, 3, 4, 5];
values.reverse();
alert(values); //5,4,3,2,1
```

#####sort()

sort()方法按升序排列数组项，为了实现排序，sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值，sort()方法比较的也是字符串。

```javascript
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values); //0,1,10,15,5 
```

sort()方法可以接收一个比较函数作为参 数，以便我们指定哪个值位于哪个值的前面。

比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等 则返回 0，如果第一个参数应该位于第二个之后则返回一个正数。

```javascript
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
} 
var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values); //0,1,5,10,15 
```

升序则只需要将函数返回值更改成相反值即可。

对于数值类型和valueof而言，有更加简便的方法

```javascript
function compare(value1, value2){
	return value2 - value1;
}
```



#### 操作方法

#####concat()

concat()方法可以基于当前数 组中的所有项创建一个新数组。具体来说，这个方法会先创建当前数组一个副本，然后将接收到的参数 添加到这个副本的末尾，最后返回新构建的数组。

```javascript
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);
alert(colors); //red,green,blue
alert(colors2); //red,green,blue,yellow,black,brown 
```

#####slice()

slice()能够基于当前数组中的一或多个项创建一个新数组。slice()方法可以 接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项— —但不包括结束位置的项。

```javascript
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);
alert(colors2); //green,blue,yellow,purple
alert(colors3); //green,blue,yellow 
```

#####splice()

splice()方法的三种用法

1. 删除。可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。
2. 插入。可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、0（要删除的项数） 和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。
3. 替换。可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起 始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。

```javascript
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1); // 删除第一项
alert(colors); // green,blue
alert(removed); // red，返回的数组中只包含一项
removed = colors.splice(1, 0, "yellow", "orange"); // 从位置 1 开始插入两项
alert(colors); // green,yellow,orange,blue
alert(removed); // 返回的是一个空数组
removed = colors.splice(1, 1, "red", "purple"); // 插入两项，删除一项
alert(colors); // green,red,purple,orange,blue
alert(removed); // yellow，返回的数组中只包含一项
```



####位置方法

indexOf()和 lastIndexOf()。这两个方法都接收 两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中，indexOf()方法从数组的开头（位 置 0）开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找。

这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(4)); //3 
alert(numbers.lastIndexOf(4)); //5
alert(numbers.indexOf(4, 4)); //5
alert(numbers.lastIndexOf(4, 4)); //3
var person = { name: "Nicholas" };
var people = [{ name: "Nicholas" }];
var morePeople = [person];
alert(people.indexOf(person)); //-1
alert(morePeople.indexOf(person)); //0 
```



####迭代方法

迭代方法有5种，每个方法都接收两个参数：要在每一项上运行的函数和 （可选的）运行该函数的作用域对象——影响 this 的值。

传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身

1. every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。 
2. filter()：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。 
3. forEach()：对数组中的每一项运行给定函数。这个方法没有返回值。 
4. map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。 
5. some()：对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
 	return (item > 2);
});
alert(everyResult); //false
var someResult = numbers.some(function(item, index, array){
 	return (item > 2);
});
alert(someResult); //true 
var filterResult = numbers.filter(function(item, index, array){
 	return (item > 2);
});
alert(filterResult); //[3,4,5,4,3] 
var mapResult = numbers.map(function(item, index, array){
 	return item * 2;
});
alert(mapResult); //[2,4,6,8,10,8,6,4,2] 
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
 //执行某些操作
});
```



#### 归并方法

reduce()和 reduceRight()都会迭 代数组的所有项，然后构建一个最终返回的值。其中，reduce()方法从数组的第一项开始，逐个遍历到最后。而reduceRight()则从数组的最后一项开始，向前遍历到第一项。

这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。传给 reduce()和 reduceRight()的函数接收 4 个参数：前一个值、当前值、项的索引和数组对象。这 个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第 一个参数是数组的第一项，第二个参数就是数组的第二项。

```javascript
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
 	return prev + cur;
});
alert(sum); //15
var sum = values.reduceRight(function(prev, cur, index, array){
	return prev + cur;
});
alert(sum); //15 
```

reduce()中第一次执行回调函数，prev 是 1，cur 是 2。第二次，prev 是 3（1 加 2 的结果），cur 是 3（数组 的第三项）。这个过程会持续到把数组中的每一项都访问一遍，最后返回结果。

reduceRight()中执行也和reduce()一致，只是方向不一样。



### Data类型

###### 创建方法

```javascript
var now = new Date(); 
```

在调用 Date 构造函数而不传递参数的情况下，新创建的对象自动获得当前日期和时间。

如果建立特定时间对象，可以通过Data.parse()和Data.parse()进行建立。

在通过Data.parse()建立日期对象时，需要接受一个符合格式的表示日期的字符串。

1. “月/日/年”，如 6/13/2004； 
2. “英文月名 日,年”，如 January 12,2004； 
3. “英文星期几 英文月名 日 年 时:分:秒 时区”，如 Tue May 25 2004 00:00:00 GMT-0700。

```javascript
var someDate = new Date(Date.parse("May 25, 2004")); 
```

实际上，直接将表达日期的字符串传入Data构造函数中也会自动调用parse()函数，也就是说可以直接通过new来建立特定时间对象

```javascript
var someDate = new Date("May 25, 2004");
```

Date.UTC()方法同样也返回表示日期的毫秒数，但它与 Date.parse()在构建值时使用不同的信 息。Date.UTC()的参数分别是年份、基于 0 的月份（一月是 0，二月是 1，以此类推）、月中的哪一天 （1 到 31）、小时数（0 到 23）、分钟、秒以及毫秒数。在这些参数中，只有前两个参数（年和月）是必需的。如果没有提供月中的天数，则假设天数为 1；如果省略其他参数，则统统假设为 0.

```javascript
// GMT 时间 2000 年 1 月 1 日午夜零时
var y2k = new Date(Date.UTC(2000, 0));
// GMT 时间 2005 年 5 月 5 日下午 5:55:55
var allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));
```



##### 日期格式化方法

专门用来表示时间的方法

1. toDateString()——以特定于实现的格式显示星期几、月、日和年； 
2. toTimeString()——以特定于实现的格式显示时、分、秒和时区； 
3. toLocaleDateString()——以特定于地区的格式显示星期几、月、日和年； 
4. toLocaleTimeString()——以特定于实现的格式显示时、分、秒；  toUTCString()——以特定于实现的格式完整的 UTC 日期。



### RegExp类型

ECMAScript 通过 RegExp 类型来支持正则表达式。

#### 创建正则表达式

#####字面变量创建

```javascript
var expression = / pattern / flags ; 
```

其中的模式（pattern）部分可以是任何简单或复杂的正则表达式，可以包含字符类、限定符、分组、 向前查找以及反向引用。每个正则表达式都可带有一或多个标志（flags），用以标明正则表达式的行为。

#####flag表达模式

1. g：表示全局（global）模式，即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止； 
2. i：表示不区分大小写（case-insensitive）模式，即在确定匹配项时忽略模式与字符串的大小写； 
3. m：表示多行（multiline）模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项。

```javascript
// 匹配字符串中所有"at"的实例
var pattern1 = /at/g;
//匹配第一个"bat"或"cat"，不区分大小写
var pattern2 = /[bc]at/i;
// 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
var pattern3 = /.at/gi; 
```



#####转义字符

与其他语言中的正则表达式类似，模式中使用的所有元字符都必须转义。正则表达式中的元字符包括： 

( [ { \ ^ $ | ) ? * + .]} 

```javascript
//匹配第一个"bat"或"cat"，不区分大小写
var pattern1 = /[bc]at/i; 
//匹配第一个" [bc]at"，不区分大小写
var pattern2 = /\[bc\]at/i;
//匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
var pattern3 = /.at/gi;
//匹配所有".at"，不区分大小写
var pattern4 = /\.at/gi; 
```



#####RegExp构造函数创建

RegExp 构造函数接收两个参数：一个是要匹配的字符串模式，另一个是可选的标志字符串。

```javascript
//匹配第一个"bat"或"cat"，不区分大小写
var pattern1 = /[bc]at/i;
//与 pattern1 相同，只不过是使用构造函数创建的
var pattern2 = new RegExp("[bc]at", "i"); 
```

由于 RegExp 构造 函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义。所有元字符都必须双重转义，那 些已经转义过的字符也是如此，例如\n（字符\在字符串中通常被转义为\\，而在正则表达式字符串中就 会变成\\\\）。



####RegExp实例

##### 实例属性

1. global：布尔值，表示是否设置了 g 标志。 
2. ignoreCase：布尔值，表示是否设置了 i 标志。 
3. lastIndex：整数，表示开始搜索下一个匹配项的字符位置，从 0 算起。 
4. multiline：布尔值，表示是否设置了 m 标志。 
5. source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。

source 属性保存的是规范形式的字符串，即字面量形式所用的字符串。



##### 实例方法

exec()接受一个参数，即 要应用模式的字符串，然后返回包含第一个匹配项信息的数组；或者在没有匹配项的情况下返回 null。返回的数组虽然是 Array 的实例，但包含两个额外的属性：index 和 input。其中，index 表示匹配 项在字符串中的位置，而 input 表示应用正则表达式的字符串。。在数组中，第一项是与整个模式匹配 的字符串，其他项是与模式中的捕获组匹配的字符串（如果模式中没有捕获组，则该数组只包含一项）。

```javascript
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;
var matches = pattern.exec(text);
alert(matches.index); // 0
alert(matches.input); // "mom and dad and baby"
alert(matches[0]); // "mom and dad and baby"
alert(matches[1]); // " and dad and baby"
alert(matches[2]); // " and baby" 
```

对于 exec()方法而言，即使在模式中设置了全局标志（g），它每次也只会返回一个匹配项。在不 设置全局标志的情况下，在同一个字符串上多次调用 exec()将始终返回第一个匹配项的信息。而在设置全局标志的情况下，每次调用 exec()则都会在字符串中继续查找新匹配项

```javascript
var text = "cat, bat, sat, fat";
var pattern1 = /.at/;
var matches = pattern1.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern1.lastIndex); //0
matches = pattern1.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern1.lastIndex); //0
var pattern2 = /.at/g;
var matches = pattern2.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern2.lastIndex); //3 
matches = pattern2.exec(text);
alert(matches.index); //5
alert(matches[0]); //bat
alert(pattern2.lastIndex); //8 
```

正则表达式的第二个方法是 test()，它接受一个字符串参数。在模式与该参数匹配的情况下返回 true；否则，返回 false。

简单来说就是用来匹配字符串格式。

```javascript
var text = "000-00-0000";
var pattern = /\d{3}-\d{2}-\d{4}/;
if (pattern.test(text)){
 	alert("The pattern was matched.");
} 
```

的 toLocaleString()和 toString()方法都会返回正则表达式的字面量。

```javascript
var pattern = new RegExp("\\[bc\\]at", "gi");
alert(pattern.toString()); // /\[bc\]at/gi
alert(pattern.toLocaleString()); // /\[bc\]at/gi 
```



### Function类型

####函数特点

1. 没有重载。JavaScript函数可以想象为指针，多次进行定义只会导致覆盖，不会进行重载
2. 函数与函数表达式。JavaScript函数分为函数声明与函数表达式，但只有函数声明有函数声明提升。
3. 函数能够作为值被传递。因为JavaScript函数本身就是变量，所以可以被当作值进行使用。

具体将在另一个文件进行详细解释



### 基础包装类型

每当读取一个基本类型值的时候，后台就会创建一个对应的基本包装类型的对象，从而让我们能够调用一些方法来操作这些数据。基础类型只是一个值，而不是对象，但是却能调用方法，其实调用方法的并不是基础类型，而是基础包装类型。它在使用方法时建立，在调用后销毁。

#### Boolean

Boolean 类型是与布尔值对应的引用类型。要创建 Boolean 对象，可以像下面这样调用 Boolean 构造函数并传入 true 或 false 值。

```javascript
var booleanObject = new Boolean(true); 
```

Boolean在应用中用处不大，因为常常有着误解。

```javascript
var falseObject = new Boolean(false);
var result = falseObject && true;
alert(result); //true
var falseValue = false;
result = falseValue && true;
alert(result); //false
```

falseObject变量在第二行代码中，它其实并不是代表他的值false，而是对象falseObject。在布尔表达式中，所有对象都会进行转化成true，所以在这里得到的结果是true。

Boolean建立的是一个对象，而不是一个布尔值，通过typeof可以得知他们的不同。其次，instanceof可以具体检验Boolean的类型。

```javascript
alert(typeof falseObject); //object
alert(typeof falseValue); //boolean
alert(falseObject instanceof Boolean); //true
alert(falseValue instanceof Boolean); //false 
```



#### Number

Number 是与数字值对应的引用类型。要创建 Number 对象，可以在调用 Number 构造函数时向其 中传递相应的数值。

```javascript
var numberObject = new Number(10); 
```

toString()方法可以传递一个代表基数的参数，从而达成转制。

```javascript
var num = 10;
alert(num.toString()); //"10"
alert(num.toString(2)); //"1010"
alert(num.toString(8)); //"12"
alert(num.toString(10)); //"10"
alert(num.toString(16)); //"a" 
```

也可以通过toFixed()方法进行格式化表示，并且可以进行四舍五入操作

```javascript
var num = 10;
alert(num.toFixed(2)); //"10.00" 
var num = 10.005;
alert(num.toFixed(2)); //"10.01" 
```

另外可用于格式化数值的方法是 toExponential()，该方法返回以指数表示法（也称 e 表示法） 表示的数值的字符串形式。

```javascript
var num = 10;
alert(num.toExponential(1)); //"1.0e+1" 
```

还有toPrecision()方法，toPrecision()会根据要处理的数值决定到底是调用 toFixed()还是调用 toExponential()。

```javascript
var num = 99;
alert(num.toPrecision(1)); //"1e+2"
alert(num.toPrecision(2)); //"99"
alert(num.toPrecision(3)); //"99.0" 
```

和Boolean一样，Number类型的 typeof 和 instanceof 的测试结果也是和number类型完全不一样。

```javascript
var numberObject = new Number(10);
var numberValue = 10;
alert(typeof numberObject); //"object"
alert(typeof numberValue); //"number"
alert(numberObject instanceof Number); //true
alert(numberValue instanceof Number); //false 
```



#### String

String 类型是字符串的对象包装类型，可以像下面这样使用 String 构造函数来创建

```javascript
var stringObject = new String("hello world"); 
```

#####字符方法

1. charAt()：接受一个参数，以单字符字符串的形式返回给定位置的那个字符
2. charCodeAt()：接受一个参数，返回给定位置的那个字符的编码

```javascript
var stringValue = "hello world";
alert(stringValue.charAt(1)); //"e" 
alert(stringValue.charCodeAt(1)); //"101" 
```

除此之外，还有直接像数组一样进行访问

```javascript
var stringValue = "hello world";
alert(stringValue[1]); //"e" 
```



#####字符串操作方法

1. concat()：用于将一或多个字符串拼接起来， 返回拼接得到的新字符串。用处不大，因为可以使用" + "来连接字符串
2. silce()：用于剪切字符串，第一个参数为开始的位置；第二个参数为结束位置，如果第二个参数为空，则截取到最后；如果第一或第二个参数为负数，取与字符串长度相加的结果
3. substring()：用于剪切字符串，第一个参数为开始位置，第二个参数为结束位置，若为空，则截取到最后，第一或第二个参数如果为负数，则取 0。如果第二个参数小于第一个参数，则交换两个参数的值
4. substr()：用于剪切字符串，第一个参数为开始位置，第二个参数为字符个数；若第一个参数为负数，则取与字符串长度相加的结果，若第二个参数为负数，则取 0。

```javascript
var stringValue = "hello world";
alert(stringValue.slice(3)); //"lo world"
alert(stringValue.substring(3)); //"lo world"
alert(stringValue.substr(3)); //"lo world"
alert(stringValue.slice(3, 7)); //"lo w"
alert(stringValue.substring(3,7)); //"lo w"
alert(stringValue.substr(3, 7)); //"lo worl" 
alert(stringValue.slice(-3)); //"rld"
alert(stringValue.substring(-3)); //"hello world"
alert(stringValue.substr(-3)); //"rld"
alert(stringValue.slice(3, -4)); //"lo w"
alert(stringValue.substring(3, -4)); //"hel"
alert(stringValue.substr(3, -4)); //""（空字符串）
```



#####字符串位置方法

1. indexof()：从字符串开头向后搜索，返回子字符串位置，若没找到，返回 -1。第一个参数为子字符串，第二个参数为开始位置。
2. lastIndexof()：从字符串末尾向前搜索，返回子字符串位置，若没找到，返回 -1。第一个参数为子字符串，第二个参数为开始位置。

```javascript
var stringValue = "hello world";
alert(stringValue.indexOf("o")); //4
alert(stringValue.lastIndexOf("o")); //7 
alert(stringValue.indexOf("o", 6)); //7
alert(stringValue.lastIndexOf("o", 6)); //4 
```



#####trim()方法

创建一个副本，删除前置和后置所有空格并返回。

```javascript
var stringValue = " hello world ";
var trimmedStringValue = stringValue.trim();
alert(stringValue); //" hello world "
alert(trimmedStringValue); //"hello world" 
```



#####大小写转换

1. toLowerCase()、toLocaleLowerCase()，将字符串返回为小写。
2. toUpperCase()、toLocaleUpperCase()，将字符串返回为大写。

其中toLocaleLowerCase()、toLocaleUpperCase()根据地区会有所不同



#####字符串模式匹配方法

1. match()：接受一个正则表达式，或者RegExp对象，返回匹配到的字符串组成的数组。

2. search()：接受一个正则表达式，或者RegExp对象，返回第一个匹配到的字符串下标。

3. replace()：接受两个参数：第一个参数可以是一个 RegExp 对象（正则表达式）或者一个字符串（这个字符串不会被转换成正则表达式），第二个参数可以是一个字符串或者一个函数。返回被替代后的字符串。如果第二个参数是字符串，那么还可以使用一些特殊的字符序列，将正则表达式操作得到的值插入 到结果字符串中。

   ![image-20200425001907533](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200425001907533.png)

4. spilt()：第一个参数分隔符可以是字符串，也可以是一个 RegExp 对象；可选的第二个参数，用于指定数组的大小。

```javascript
var text = "cat, bat, sat, fat";
var pattern = /.at/;
//与 pattern.exec(text)相同
var matches = text.match(pattern);
alert(matches.index); //0
alert(matches[0]); //"cat"
alert(pattern.lastIndex); //0 
var pos = text.search(/at/);
alert(pos); //1 
var result = text.replace("at", "ond");
alert(result); //"cond, bat, sat, fat"
result = text.replace(/at/g, "ond");
alert(result); //"cond, bond, sond, fond" 
result = text.replace(/(.at)/g, "word ($1)");
alert(result); //word (cat), word (bat), word (sat), word (fat) 
var colorText = "red,blue,green,yellow";
var colors1 = colorText.split(","); //["red", "blue", "green", "yellow"]
var colors2 = colorText.split(",", 2); //["red", "blue"]
var colors3 = colorText.split(/[^\,]+/); //["", ",", ",", ",", ""] 
```



#####localCompare()

这个方法比较两个字符串，并返回下列 值中的一个： 

1. 如果字符串在字母表中应该排在字符串参数之前，则返回一个负数（大多数情况下是-1，具体 的值要视实现而定）； 
2. 如果字符串等于字符串参数，则返回 0； 
3. 如果字符串在字母表中应该排在字符串参数之后，则返回一个正数（大多数情况下是 1，具体的 值同样要视实现而定）。

```javascript
var stringValue = "yellow";
alert(stringValue.localeCompare("brick")); //1
alert(stringValue.localeCompare("yellow")); //0
alert(stringValue.localeCompare("zoo")); //-1 
```



###单体内置对象

#### Global对象

ECMAScript 中的 Global 对象在某种意义上是作为一个终极的“兜底儿对象” 来定义的。换句话说，不属于任何其他对象的属性和方法，最终都是它的属性和方法。事实上，没有全 局变量或全局函数；所有在全局作用域中定义的属性和函数，都是 Global 对象的属性。

#####URI编码方法

1. encodeURI()：用于对整个URI进行编码。encodeURI()不会对本身属于 URI 的特殊字符进行编码，
2. encodeURIComponent()：用于对URI某一段进行编码。encodeURIComponent()则会对它发现的任何非标准字符进行编码。
3. decodeURI()：与encodeURI()对应，进行解码时不能解%23，因为为%23 表示井字号（#），而井字号不是使用 encodeURI()替换的。
4. decodeURIComponent()：与encodeURIComponent()对应，可以进行任何解码。

```javascript
var uri = "http://www.wrox.com/illegal value.htm#start";
alert(encodeURI(uri));//"http://www.wrox.com/illegal%20value.htm#start"
alert(encodeURIComponent(uri)); //"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start"

var uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start";
alert(decodeURI(uri));//http%3A%2F%2Fwww.wrox.com%2Fillegal value.htm%23start
alert(decodeURIComponent(uri)); //http://www.wrox.com/illegal value.htm#start
```

#####eval()方法

它只接受一个参数，即要执行的 ECMAScript（或 JavaScript） 字符串。

当解析器发现代码中调用 eval()方法时，它会将传入的参数当作实际的 ECMAScript 语句来解析， 然后把执行结果插入到原位置。通过 eval()执行的代码被认为是包含该次调用的执行环境的一部分， 因此被执行的代码具有与该执行环境相同的作用域链。这意味着通过 eval()执行的代码可以引用在包含环境中定义的变量。

```javascript
var msg = "hello world";
eval("alert(msg)"); //"hello world" 
eval("function sayHi() { alert('hi'); }");
sayHi();
```

 eval()中创建的任何变量或函数都不会被提升，因为在解析代码的时候，它们被包含在一个字 符串中；它们只在 eval()执行的时候创建。

 严格模式下，在外部访问不到 eval()中创建的任何变量或函数，因此前面两个例子都会导致错误。



#### Math对象

#####Math对象属性

![image-20200425012056960](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200425012056960.png)

#####min()和max()方法

min()和 max()方法用于确定一组数值中的最小值和最大值。这两个方法都可以接收任意多 个数值参数。也可以通过apply()进行查找数组的最值

```javascript
var max = Math.max(3, 54, 32, 16);
alert(max); //54
var min = Math.min(3, 54, 32, 16);
alert(min); //3 
var values = [1, 2, 3, 4, 5, 6, 7, 8];
var max = Math.max.apply(Math, values);
```



#####舍入方法

1. Math.ceil()执行向上舍入，即它总是将数值向上舍入为最接近的整数； 
2. Math.floor()执行向下舍入，即它总是将数值向下舍入为最接近的整数； 
3. Math.round()执行标准舍入，即它总是将数值四舍五入为最接近的整数

```javascript
alert(Math.ceil(25.9)); //26
alert(Math.ceil(25.5)); //26
alert(Math.ceil(25.1)); //26
alert(Math.round(25.9)); //26
alert(Math.round(25.5)); //26
alert(Math.round(25.1)); //25

alert(Math.floor(25.9)); //25
alert(Math.floor(25.5)); //25
alert(Math.floor(25.1)); //25 
```



#####random()方法

Math.random()方法返回大于等于 0 小于 1 的一个随机数。可以通过公式从范围中取值

```javascript
function selectFrom(lowerValue, upperValue) {
     var choices = upperValue - lowerValue + 1;
     return Math.floor(Math.random() * choices + lowerValue);
}
var num = selectFrom(2, 10);
alert(num); // 介于 2 和 10 之间（包括 2 和 10）的一个数值
```



#####其他方法

![image-20200425013039303](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20200425013039303.png)