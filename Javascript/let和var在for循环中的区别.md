## `let`和`var`在`for`循环中的区别

### `var- for`

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i)//输出全为3
    }, 0)
}
console.log(i)//输出为3
```

`setTimeout`函数为异步函数，在主线程执行后调用，所以输出的结果为循环结束后的值



### `let-for`

```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i)//0,1,2
    }, 0)
}
console.log(i)//undefined
```

在`let-for`中，每次循环都会生成一个块作用域，里面`let`会使`i`值重新生成，不受上一次循环的影响，所以即使是异步函数，也不会产生`i`值全部相同的结果。