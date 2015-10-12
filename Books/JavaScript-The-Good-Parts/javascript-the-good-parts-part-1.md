# JavaScript: The Good Parts - Part 1

## Block Comments

我们可能常常会使用 `/*  */` 来注释我们的多行代码，但是有的时候我们的 JavaScript 代码中也会含有 `*` `/` 这样的符号，所以块级注释就有可能不安全，例如我们的代码中还有正则表达式的字面量表示的时候

```javascript
/*
    var rm_a = /a*/.match(s);
*/
```
## % - Remainder Operation or Modulo Operation

JavaScript 中的 `%` 是求余，而并非求模

C/Java 也是求余，但是 Python 是求模

### Difference Between Them

对于两个正数或者两个负数进行取余或取模运算，结果是一样的，但若其中一个为负数，则有不同

首先，取余或取模的方法其实都一样，方法如下:

1. c = a / b (求 a 除以 b 的正数商)
2. r = a - b * c (求余或求模 r)

但是，对于求余和求模来说，正数商却是不一样的。准确来说，是当其中一个数为负数的时候，正数商是不一样的。

求余的时候，整数商是向 0 取整，求模的时候，整数商是向下取整，例如:

(-13) % 64 时，求余的整数商是 0，求模的整数商是 －1，所以得出来的结果求余为 -13，求模为 51

那要怎么在 JavaScript 中实现模运算呢，我们可以为 `Math` 对象定义一个方法:

```javascript
Math.prototype.mod = funciton (n, m) {
    return ((n % m) + m) % m;
}
```

### More Reading

- [Javascript modulo not behaving](http://stackoverflow.com/questions/4467539/javascript-modulo-not-behaving)
- [取余运算区别](http://baike.baidu.com/view/4887065.htm#1)
- [负数取模怎么算](http://www.jianshu.com/p/452c1a5acd31)


## Object ＆ Functions

用 `{ }` 和 `new Object()` 创建的对象，都会连接到 `Object.prototype`，这里说的连接是通过 `__proto__` 属性来连接

每个函数都是对象，它们也会通过 `__proto__` 来连接到 `Function.prototype`，而 `Function.prototype` 也会通过 `__proto__` 来连接到 `Object.prototype`

与此同时，每个函数在创建的时候也会分配到一个 `prototype` 属性，但 `prototype` 和 `__proto__` 完全不同

### Invocation 

JavaScript 中的函数有四种不同的调用模式，详情可以看 [This in Function](https://l-movingon.github.io/posts/2015-09-09-this-in-function.html)

更正一点，把函数当成构造器调用的时候，如果函数里有返回值，并且这个返回值是一个对象的时候，这个返回值才有效。换句话说，如果返回值是数字，字符串或者是布尔值的时候，返回值会被忽略，返回的依旧是 `this`，也就是新创建出来的对象

## Scope

> JavaScript has Global Scope(Winodw) and Function Scope before ES6

JavaScript 不像其他类 C 语言一样有块级作用域，这意味着在一个函数内部定义的变量，对于函数的任何都可见

因为 JavaScript 中有 [Hoisting](https://l-movingon.github.io/posts/2015-03-05-about-hoisting.html) 的概念

## let and const 

```javascript
var foo = 1;

function fun () {
    var baz = 2;
    if (x) {
        var z = 4;
        let y = 3;
    }
}

// Global Scope: foo
// Function Scope: baz z
// Block Scope: y
```

## Closure

闭包的使用场景有很多，例如在 JavaScript 中实现公有变量，私有变量，这有助于模块化我们的代码，减少我们的全局变量,还可以模拟块级作用域

我想所以有讲闭包的时候，都会举一个类似的这样的例子

```javascript
var arrs = [];

for (var i = 1; i <= 3; i++) {
    arrs.push(function () {
        console.log(i);
    });
}

arrs.forEach(function (fn) {
    fn();
}

// 然后书上会说，人们期待的结果是 1 2 3 
// 但是出来的却是 3 3 3
```

然后我们就可以用闭包去解决这个问题

```javascript
var arrs = [];

for (var i = 1; i <= 3; i++) {
    arrs.push((function () {
        return function () {
            console.log(i);
        }
    })(i));
}

arrs.forEach(function (fn) {
    fn();
}

// 1 2 3 
```

简单的说，就是在返回函数的外部函数，在自执行的时候传递了参数 `i`，这样一来，返回函数中沿着作用域链向上找 `i` 的时候，找到的就是传起来的参数 `i`，所以打印出来的自然是 1 2 3

而之所以一开始打印的是 3 3 3，是因为函数里引用的 `i` 顺着作用域链找上去的是全局变量 `i`，而全局的变量 `i` 在循环之后就变成了 3，所以有这样的结果。

想了解这两个例子的作用域链也可以看 [这里](https://l-movingon.github.io/posts/2015-01-12-about-closures.html)

那如果我们有了块级作用域，那么问题也不存在了，每次函数的引用则都是 `for` 循环里的 `i`，所以每次都会不一样了

```javascript
var arrs = [];

for (let i = 1; i <= 3; i++) {
    arrs.push(function () {
        console.log(i);
    });
}

arrs.forEach(function (fn) {
    fn();
}

// 1 2 3
```

模块化通常结合单例模式一起用，具体可以看 [Singleton Pattern in JavaScript](https://l-movingon.github.io/posts/2015-03-11-singleton-pattern.html)