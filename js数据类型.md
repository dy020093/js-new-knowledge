## 一、数据类型
### 1. JavaScript有哪些数据类型，它们的区别？
    js中的数据类型氛围两种：基本数据类型和引用数据类型。
    基本数据类型： Number、String、Boolean、Null和Undefind。
    引用数据类型： Object（Object、Array、Function、Data等）
    在es6中新增了 Symbol 和 BigInt两种数据类型。
    Symbol 代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题
    BigInt 是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了 Number 能够表示的安全整数范围。
- 基本数据类型是存储到栈中
    存储到栈中的数据是简单数据，占据的空间大小是固定的。
- 引用数据类型是存储到堆中
    存储到堆中的数据是复杂数据，占据的空间大小不固定。引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

### 2.数据类型检测
**（1）typeof**
```javascript
    console.log(typeof 2);               // number
    console.log(typeof true);            // boolean
    console.log(typeof 'str');           // string
    console.log(typeof []);              // object    
    console.log(typeof function(){});    // function
    console.log(typeof {});              // object
    console.log(typeof undefined);       // undefined
    console.log(typeof null);            // object
    console.log(typeof NaN);            // number
```
    其中基本数据类型会被直接识别（除null外），引用数据类型中的数组、对象会被识别为object
    js的底层是二进制，而000的数据会被判断为object。而null的二进制表示全为0，所以在typeof的判断时显示object
    NaN 指“不是一个数字”（not a number），NaN 是一个“警戒值”（sentinel value，有特殊用途的常规值），用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。
    NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 `x === x` 不成立）的值。而 `NaN !== NaN` 为 true。
- 函数 isNaN 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断。
- 函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。
**（2）instanceof**
`instanceof`可以正确判断对象的类型，**其内部运行机制是判断在其原型链中能否找到该类型的原型**。

```javascript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

可以看到，`instanceof`**只能正确判断引用数据类型**，而不能判断基本数据类型。`instanceof` 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype` 属性。

**（3） constructor**

```javascript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
```

`constructor`有两个作用，一是判断数据的类型，二是对象实例通过 `constructor` 对象访问它的构造函数。需要注意，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了：

```javascript
function Fn(){};
 
Fn.prototype = new Array();
 
var f = new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```

### 3. null和undefined区别
  null和undefined都是基本数据类型，也都只有一个值。
  undefined的含义是未找到，未定义，变量未声明。null的含义是空值，变量声明未赋值
  undefined在js中不是保留字，可以设置变量名。但是一般不建议。
  null和undefined使用`==`判断时返回true,用`===`判断时返回false

### 4. intanceof 操作符的实现原理及实现

 instanceof 运算符用于判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。

```javascript
function myInstanceof(left, right) {
  // 获取对象的原型
  let proto = Object.getPrototypeOf(left)
  // 获取构造函数的 prototype 对象
  let prototype = right.prototype; 
 
  // 判断构造函数的 prototype 对象是否在对象的原型链上
  while (true) {
    if (!proto) return false;
    if (proto === prototype) return true;
    // 如果没有找到，就继续从其原型上找，Object.getPrototypeOf方法用来获取指定对象的原型
    proto = Object.getPrototypeOf(proto);
  }
}
```
### 5. 类型转换
**显示类型转换**
显示类型转换是由js提供的一些函数，直接进行的转换。

- 转化为Nnumber类型： Number() / parseFloat() / parseInt()
- 转化为 String 类型：String() / toString()
- 转化为 Boolean 类型: Boolean()
值得注意的是undefined、null、false、+0、-0、NaN、""  只有这些 toBoolean()  是 false ，其余都为 true

**隐式类型转换**
隐式类型转换主要涉及两个操作符`+`和`==`

`+`中的转换： 
```javascript
  console.log('123'+'123')   // '123123'
  console.log(123+'123')     // '123123'
  console.log('123'+123)     // '123123'
  console.log(123+123)       // 246
  console.log(true+'123')    // true123
  console.log(true+123)      // 124
  console.log(null+'123')    // 'null123'
  console.log(null+123)      // 123
  console.log(undefined+123)  // NaN
```
（1）两个字符串间的相加直接拼接成一个新的字符串。任何类型的值与字符串相加都会先被转换为字符串在相加。
（2）两个数字相加,直接输出为算数相加。
（3）布尔值与数字相加，会将布尔值转换数字在算数运算。null与布尔相同，null转为0
（4）undefined与数字相加，输出为NaN。虽然输出为NaN，但undefined也进行了数字类型转换，转换为了NaN，因为NaN与任何数字相加都为NaN，所以输出为NaN
`==`中的类型转换
```javascript
  console.log(NaN == NaN)   // false
  console.log(true == 1)    // true
  console.log(true == '1')    // true
  console.log('1' == true)    // true
  console.log('123' == 123)    // true
  console.log(null == undefined)    // true
  console.log(null == 0)    // false
```
（1）NaN不等于任何值，包括他自身。
（2）布尔值与其他值进行比较都会先转为数字。
（3）字符串与数字进行比较会先转为数字。
（4）null 与 undefined进行比较结果为true。null与undefined在比较的时候都会转换数字0。
（5）null,undefined与其它任何类型进行比较结果都为false。
（6）引用类型与引用类型，直接判断是否指向同一对象
（7）引用类型与值类型进行比较,引用类型先转换为值类型(调用[ToPrimitive])