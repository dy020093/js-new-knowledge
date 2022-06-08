##  1. let、const、var的区别
（1）let、const没有变量提升，在声明之前无法获取声明的变量。var有变量提升，可以在声明之前获取到变量。同时这种现象被称为暂时性死区。
（2）块级作用域，`{}`包括的区域，let、const在`{}`中生命的变量只能在其中使用，而var声明的变量可以在全局使用。
（3）重复声明，var可以重复声明同一个变量名，但是let、const不可以，重复声明会报错。
（4）var和let可以随意修改变量，const不能修改变量内存地址。如cosnt声明的是基本数据类型，那这个变量不可被修改，声明的是复杂数据类型，这个变量类型不能被修改，只能修改这个内存地址中的数据。

## 2.箭头函数
箭头函数是ES6中新增的，没有prototype，没有自己的this指向，不能使用arguments参数。即不能使用new来创造。
###箭头函数与普通函数的区别
（1）箭头函数更简洁
- 如果没有参数，就直接写一个空括号即可
- 如果只有一个参数，可以省去参数的括号
- 如果有多个参数，用逗号分割
- 如果函数体的返回值只有一句，可以省略大括号
- 如果函数体不需要返回值，且只有一句话，可以给这个语句前面加一个void关键字。最常见的就是调用一个函数：

```javascript
let fn = () => void doesNotReturn();
```
（2）箭头函数没有自己的this,在箭头函数中的this永远指向上一级
（3）call、apply、bind无法强制修改箭头函数中this的指向
###### 箭头函数指向哪里
```javascript
// ES6 
const obj = { 
  getArrow() { 
    return () => { 
      console.log(this === obj); 
    }; 
  } 
}
```

转化后：

```javascript
// ES5，由 Babel 转译
var obj = { 
   getArrow: function getArrow() { 
     var _this = this; 
     return function () { 
        console.log(_this === obj); 
     }; 
   } 
};
```
## 3、拓展运算符
（1）对象的拓展运算符。
```javascript
    const obj1 = {
        a: 1,
        b: 2
    }
    const obj2 = {
        c: 3
    }
    const obj = {...obj1, ...obj2}
    obj = {
        a: 1,
        b: 2,
        c: 3
    }
```
对象的拓展运算相当于`Object.assign`，合并时有相同的属性名，在后来的属性名会覆盖之前的属性名。
拓展运算属于浅拷贝。
（2）数组的拓展运算

- **展开二维数组**
```javascript
console.log(...[1, 2, 3])
// 1 2 3
console.log(...[1, [2, 3, 4], 5])
// 1 [2, 3, 4] 5
```
- **将数组转换为参数序列**

```javascript
function add(x, y) {
  return x + y;
}
const numbers = [1, 2];
add(...numbers) // 3
```
- **复制数组/合并数组**

```javascript
const arr1 = [1, 2];
const arr2 = [...arr1];
const arr1 = ['two', 'three'];
const arr2 = ['one', ...arr1, 'four', 'five'];
// ["one", "two", "three", "four", "five"]
```
- **将字符串转为真正的数组**

```javascript
[...'hello']    // [ "h", "e", "l", "l", "o" ]
```

- **使用**`Math`**函数获取数组中最大最小**

```javascript
const numbers = [9, 4, 7, 1];
Math.min(...numbers); // 1
Math.max(...numbers); // 9
```
## 4、模版语法
在模版语法之前拼接字符串是一个很麻烦的事情。但是有了模版语法之后，字符串容易拼接了，可读性提高了，整体代码质量页变高了。另外在模版字符串有两个优势：
- 在模版字符串中，空格、锁紧、换行都会被保留
- 模版字符串支持“运算”式的表达式，可以在${}中进行计算
基于第一点，可以在模板字符串里无障碍地直接写 html 代码：

```javascript
let list = `
    <ul>
        <li>列表项1</li>
        <li>列表项2</li>
    </ul>
`;
console.log(message); // 正确输出，不存在报错
```
基于第二点，可以把一些简单的计算和调用丢进 ${} 来做：

```javascript
function add(a, b) {
  const finalString = `${a} + ${b} = ${a+b}`
  console.log(finalString)
}
add(1, 2) // 输出 '1 + 2 = 3'
```

## 5、判断字符串的包含关系
- **includes**：判断字符串与子串的包含关系
- **startsWith**： 判断字符串是否以某个/某串字符开头
- **endsWith**： 判断字符串是否以某个/某串字符结尾