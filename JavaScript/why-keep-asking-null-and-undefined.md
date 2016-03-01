# Why Keep Asking *null* and *undefined*

面试常常从 "JavaScript 中都有哪些数据类型" 问起，那我们的回答就会是

基本数据类型: string，boolean，number，undefined，null
引用数据类型: object(array，date，regexp，function)

然后就开始问，null 和 undefined 有什么区别，怎么检测

## What is the difference between the *null* and *undefined*

`undefined` 一般出现在

- 变量声明了却没有赋值时
- 一个函数没有 `return` 语句的时候的返回值
- 一个函数的 `return` 语句没有返回任务东西的时候
- 函数调用没有传入形参时
- 访问一个对象不存在的属性时
- 全局变量 `undefined` 的值

`null` 则是一个空对象的引用，是一个对象的点位符

[ToBoolean](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-toboolean) 显示 `null` 和 `undefined` 都会转成 `false`，而 [ToNumber](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-tonumber) `null` 会转成 0，而 `undefined` 则转成 NaN

> Q: when I want to unset the values of properties no longer in use but which I don't want to delete. Should I use null instead?

> A: Yes. Keep 'undefined' as a special value for signalling when other languages might throw an exception instead.

- `undeclared` 变量不存在
- `undefined` 变量不存在或者存在却没有赋值
- `null` 变量存在并且赋值为 null

## How do I check a variable if it's *null* or *undefined*

我们先来看看几个诡异的事实，我们再从中得出正解判断 `null` 和 `undefined` 的方法

```javascript
console.log(null == undefined); // true
console.log(null === undefined); // false

console.log(typeof null); // "object"
console.log(typeof undefined); // "undefined"
```

所以，我们判断一个变量的值是否为 `null` 的时候，只有一种方式，就是 `if (a === null)`，你不能用 `==` 是因为当 `a` 为 `undefined` 的时候，也会进入循环

而判断 `undefined` 的时候，则有两种方式，`if (a === undefined)` 或者 `if (typeof a === 'undefined')`，第一种方式不能用 `==` 的原因同上

## What is the difference between "==" and "==="...

`==` 会在对比之前，强制转换对比的两者，而 `===` 则必须数值和类型都相同

那强制转换的时候，怎么转换呢

- 当数字和字符串对比的时候，字符串会转换成对应的数值
- 当布尔在比较的时候，如果 `true` 就转换成 1，`false` 则转成 0
- 当对象与数字或者字符串对比的时候，对象会转换成 `.valueOf()` 或 `.toString()`
- 当对比两个对象的时候，对象不会转换，只有当这两个对象都指向同一个对象的时候才相等

类型的转换有点复杂，所以在比较的时候，尽量都使用 `===`