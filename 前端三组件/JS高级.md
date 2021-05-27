## 一、类型转换Number

### 1.转换方式一：

```js
// 转换方式一 Number函数 
a = Number(a)

console.log(typeof a)
```

Number() 函数的特点： 

- 纯数字字符串直接转换成数字
- 字符串有非数字内容转换为NaN
- 字符串如果是一个空串或者空字符串转换成0
- true转换成1， false转换成0
- null转换成0
- undefined转换成NaN

### 2.转换方式二：

```js
parseInt() 
parseFloat()

// var a ='123px'
// parseInt(a) // a=123
```

parseInt() 函数特点: 

- 从左到右读取到第一个非数字地方为止
- 对非String使用该函数会先转换成String, true = 1

### 3、其他进制的数字

> 在js中，十六进制用0x开头，八进制以0开头，二进制以0b开头，但是不是所有的浏览器都支持

有些浏览器会将`070`识别为八进制

```js
a = parseInt(a, 10) // 转换成十进制的数
```

### 4.将其他的数据类型转换成Boolean

调用Boolean()函数进行转换

- 数字--->布尔
  - 除了0和NaN，其余的都是true
- 字符串--->布尔
  - 除了空串其余都是true
- null和undefined都是false



## 二、原型

**1.函数的显示原型指向的对象默认是Object()实例对象(但是Object不满足)**
**2.所有函数都都是Function的实例（包含Function）**
**3.Object原型对象是原型链的尽头**
**对象的隐式原型的值默认是他对应的构造函数显示原型的值**

