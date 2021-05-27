# 1.Object.create()

**`Object.create()`**方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 （请打开浏览器控制台以查看运行结果。）

## 语法

```
Object.create(proto，[propertiesObject])
```

### 参数

- `proto`

  新创建对象的原型对象。

- `propertiesObject`

  可选。需要传入一个对象，该对象的属性类型参照[`Object.defineProperties()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)的第二个参数。如果该参数被指定且不为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)，该传入对象的自有可枚举属性(即其自身定义的属性，而不是其原型链上的枚举属性)将为新创建的对象添加指定的属性值和对应的属性描述符。

### 返回值

一个新对象，带着指定的原型对象和属性。



# 2.Object.defineProperties()

**`Object.defineProperties()`** 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

## 语法

```
Object.defineProperties(obj, props)
```

### 参数

- `obj`

  在其上定义或修改属性的对象。

- `props`

  要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅[`Object.defineProperty()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)）。描述符具有以下键：

  - `configurable``true` 只有该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。 **默认为 `false`**
  
  - `enumerable``true` 只有在枚举相应对象上的属性时该属性显现。 **默认为 `false`**
  
  - `value`与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。 **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined).**`writable``true`只有与该属性相关联的值被[assignment operator](https://developer.mozilla.org/zh-CN/docs/conflicting/Web/JavaScript/Reference/Operators_8d54701de06af40a7c984517cbe87b3e)改变时。 **默认为 `false`**
  
  - `get`作为该属性的 getter 函数，如果没有 getter 则为[](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。函数返回值将被用作属性的值。 **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**
  
  - `set`作为属性的 setter 函数，如果没有 setter 则为[`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。函数将仅接受参数赋值给该属性的新值。 **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**

### 返回值

传递给函数的对象。

# 3.Object.setPrototypeOf()

**Object.setPrototypeOf()** 方法设置一个指定的对象的原型 ( 即, 内部[[Prototype]]属性）到另一个对象或  [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)。

## 语法

```
Object.setPrototypeOf(obj, prototype)
```

### 参数

- obj

  要设置其原型的对象。.

- prototype

  该对象的新原型(一个对象 或 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)).