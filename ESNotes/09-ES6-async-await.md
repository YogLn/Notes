Promise 对象用于表示一个异步操作的最终状态（完成或失败），以及其返回的值。

`Promise` 对象是由关键字 `new` 及其构造函数来创建的。构造函数会，把一个叫做“处理器函数”（executor function）的函数作为它的参数。这个“处理器函数”接受两个函数`resolve` 和 `reject` 作为其参数。当异步任务顺利完成且返回结果值时，会调用 `resolve` 函数，而当异步任务失败且返回失败原因（通常是一个错误对象）时，会调用`reject` 函数。

**promise 状态**

　　　　pending：初始状态，既不是成功，也不是失败状态 　　　fulfilled：操作成功 　　　rejected：操作失败

## promise demo

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1    var promise1 = new Promise(function(resolve, reject) {
 2         setTimeout(function() {
 3             resolve('foo');
 4         }, 300);
 5     });
 6     promise1.then(function(value) {
 7         console.log(value);
 8         // foo
 9     });
10     console.log(promise1);
11     //  [object Promise]
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

通过外部then() 方法来绑定成功、失败的回调函数，有没有感觉这个跟之前的ajax 差不多，只不过是我们把回调丢到了then() 中，这个then 并且支持链式操作，即如果存在多个嵌套那么也就是不断的then()。

# async await 字面理解

　　先从字面意思来理解，async 是“异步”的意思，而 await 是等待的意思。所以应该很好理解 async 用于申明一个 异步的function（实际上是async function 对象），而 await 用于等待一个异步任务执行完成的的结果。

并且 await 只能出现在 async 函数中。

# async、await 如何执行

async 告诉程序这是一个异步操作，await 是一个操作符，即 await 后面是一个表达式。

async 的返回值

```
1  // async
2     async function testAsync() {
3         return "hello async";
4     }
5     const data = testAsync();
6     console.log(data);
```

如图所示：![img](https://img2018.cnblogs.com/blog/825196/201809/825196-20180916175532787-1675892261.png)

当调用一个 async 函数时，会返回一个 Promise 对象。根据mdn的解释

当这个 async 函数返回一个值时，Promise 的 resolve 方法会负责传递这个值；

当 async 函数抛出异常时，Promise 的 reject 方法也会传递这个异常值。async 函数中可能会有 await 表达式，await表达式会使 async 函数暂停执行，直到表达式中的 Promise 解析完成后继续     执行 async中await 后面的代码并返回解决结果。

**注意， await 关键字仅仅在 async function中有效**

既然返回的是Promise 对象，所以在最外层不能用 await 获取其返回值的情况下，那么肯定可以用原来的方式：then() 链来处理这个 Promise 对象 如

```
1     // async
2     async function testAsync() {
3         return "hello async";
4     }
5     let data = testAsync().then( (data) => {
6         console.log(data) // hello async                                                 
7         return data
8     });
9     console.log(data);
```

如果 async 函数没有返回值，又怎么样呢？很容易想到，它会返回 Promise.resolve(undefined)。

联想一下 Promise 的特点无等待，所以在没有 await 的情况下执行 async 函数，它会立即执行，返回一个 Promise 对象，并且，绝不会阻塞后面的语句。

# await 操作符

MDN 是这样描述 await 的：

await 表达式会暂停当前 async function 的执行，等待 Promise 处理完成。若 Promise 正常处理(fulfilled)，其回调的resolve函数参数作为 await 表达式的值，继续执行async function。若 Promise 处理异常(rejected)，await 表达式会把 Promise 的异常原因抛出。另外，如果 await 操作符后的表达式的值不是一个 Promise，则返回该值本身。

```
async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。
```

按照mdn解释 await会暂停当前async 函数执行，并且await 后面是一个表达式，即这个await 等待的是一个表达式（这个表达式返回promise 对象或者一个具体的值）：

- 假如这个表达式如果返回的是一个Promise 对象， 那么它的返回值，实际上就是 Promise 的回调函数 resolve 的参数，如果这个Promise rejected 了，await 表达式会把 Promise 的异常抛出。
- 假如这个表达式如果返回的是一个常量，那么会把这个常量转为Promise.resolve(xx)，同理如果没有返回值也是Promise.resolve(underfind)

```
1 async function testAwait() {
2          const data = await "hello await";
3          console.log(data)                                          
4         return data
5 }
```

输出 “hello await”

**返回promose 对象，成功状态**

```
 1     function say() {
 2         return new Promise(function(resolve, reject) {
 3             setTimeout(function() {
 4                 let age = 26
 5                 resolve(`hello, joel。今年我 ${age} 岁`);
 6             }, 1000);
 7         });
 8     }
 9 
10     async function demo() {
11         const v = await say(); // 输出：hello, joel。今年我 26 岁  等待这个say 的异步，如果成功把回调 resole 函数的参数作为结果
12         console.log(v);
13     }
14     demo();
```

**返回promise 对象，失败状态**

```
 1 function say() {
 2         return new Promise(function(resolve, reject) {
 3             setTimeout(function() {
 4                 let age = 26
 5                 reject(`hello, joel，发生了异常。今年我 ${age} 岁`);
 6             }, 1000);
 7         });
 8     }
 9     async function demo() {
10         try {
11             const v = await say(); // 输出：hello, joel，发生了异常。今年我 26 岁  等待这个say 的异步，如果成功把回调 resole 函数的参数作为结果
12             console.log(v);
13         } catch (e) {
14             console.log(e)
15         }
16     }
17     demo();
```

 async/await 相比原来的Promise的优势在于处理 then 链，不必把回调嵌套在then中，只要await 即可，如

```
 1     function sing() {
 2         return new Promise(function(resolve, reject) {
 3             setTimeout(function() {
 4                 resolve(`来一首好听的歌吧~~~`);
 5             }, 1000);
 6         });
 7     }
 8     async function demo() {
 9         try {
10             const v = await say(); 
11             const s = await sing(); 
12             console.log(v); // 输出：hello, joel。今年我 26 岁
13             console.log(s) // 来一首好听的歌吧~~~
14         } catch (e) {
15             console.log(e)
16         }
17     }
18     demo();
```

如果使用原来的Promise 就是把回调放在then()中。

# 总结

1. async 告诉程序这是一个异步，awiat 会暂停执行async中的代码，等待await 表达式后面的结果，跳过async 函数，继续执行后面代码
2. async 函数会返回一个Promise 对象，那么当 async 函数返回一个值时，Promise 的 resolve 方法会负责传递这个值；当 async 函数抛出异常时，Promise 的 reject 方法也会传递这个异常值
3. await 操作符用于等待一个Promise 对象，并且返回 Promise 对象的处理结果（成功把resolve 函数参数作为await 表达式的值），如果等待的不是 Promise 对象，则用 Pro