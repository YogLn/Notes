### 代理 Proxy

Proxy这个词相信你已经听过无数遍了

#### 语法

```
    let p = new Proxy(target, handler);
```

target：一个目标对象(可以是任何类型的对象，包括本机数组，函数，甚至另一个代理)用Proxy来包装。 handler：一个对象，其属性是当执行一个操作时定义代理的行为的函数。

#### 代理的使用

**基础demo：**Proxy的demo有很多，我们只分析基础demo，主要看new Proxy({}, handler)的操作，指定目标obj对象，然后handler对象执行get()操作，get()返回值的判断是，如果name是target目标对象的属性，则返回target[name]的值，否则返回37，最后测试的时候，p.a是对象p的key，所以返回a的value，而p.b不存在，返回37。

```
    const obj = {
      a: 10
    }
    let handler = {
        get: function(target, name){
            console.log('test: ', target, name)
            // test:  {"a":10} a
            // test:  {"a":10} b
            return name in target ? target[name] : 37
        }
    }
    let p = new Proxy(obj, handler)
    console.log(p.a, p.b) // 10 37
```

这个例子的作用是拦截目标对象obj，当执行obj的读写操作时，进入handler函数进行判断，如果读取的key不存在，则返回默认值。