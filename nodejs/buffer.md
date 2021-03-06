## Buffer

### Buffer对象

Buffer对象类似于数组，元素为两位16进制数，当我们建立一个Buffer空间但不赋值时，他的每个元素都是随机数。

当Buffer元素赋值少于0，则逐次加256，直到得到一个0~255的数值；

当大于255时，则逐次减少256，得到一个0~255的数值。

如果是小数，则舍弃小数部分，只保留整数。



### Buffer内存分配

Buffer使用的是`C++`层面实现的内存申请，属于堆外内存，不占用`node`的内存

Buffer内存分配采用了`slab`分配机制

**对于小于8kb的对象**，会存放在同一个8kb的`slab`单元，直到`slab`单元内存不足以放下下一个对象，则开启新的`slab`单元，并且上一个的`slab`单元剩下的空间将被浪费。在进行回收时，只有`slab`内所有的对象都被回收，才会回收`slab`单元。

**对于大于8kb的对象**，会直接分配一个`SlowBuffer`对象作为`slab`单元，这个单元将会被这个对象独占。



### Buffer转换

**字符串转Buffer**：通过`buf.write()`

**Buffer转字符串**：通过`buf.toString([encoding])`

在进行拼接时，需要注意Buffer的输出限制，不然可能会出现乱码，但是可以通过`setEncoding`进行解决，还有的话就是`decoder`也能处理这个问题。