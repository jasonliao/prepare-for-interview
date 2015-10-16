# Functional Programming

Table of Content

- [First Class Functions](#first-class-functions)
- [Pure	Functions](#pure-functions)
- [What can *pure functions* do](#what-can-pure-functions-do)
- [Curry](#curry)

## First Class Functions

当我们说函数是第一类对象的时候，并不意味着函数有多特别，相反，我们认为函数并不特别，你可以把它当成任一类型地进行存储，传参，或者赋值

没错，我们都知道，但是却经常可以看到这些多此一举的代码

```javascript
var hi = function (name) {
    return 'Hi' + name;
};

var greeting = function (name) {
    return hi(name);
};
```

因为函数是可以赋值的，所以我们可以直接把 `hi` 赋给 `greeting`

```javascript
var greeting = hi;
```

再来看看一个例子(代码摘自npm)

```javascript
var getServerStuff = function (callback) {
    return ajaxCall(function (json) {
        return callback(json);
    });
};

// 以上代码等价于
var getServerStuff = ajaxCall;
```

一开始我理解不了，为什么会等价，后来搞清楚后才真正发现，这样写代码不仅仅增加代码量，还对日后的维护困难度和代码的可读性都大大降低

这里解释一下为什么等价

```javascript
// 先看看 ajaxCall 里的那个匿名函数
function (json) {
    return callback(json);
}

// 我们为这个匿名函数加个名字为 noName，即
var noName = function (json) {
    return callback(json);
}

// 当我们调用这个 noName 函数的时候，返回的是 callback(json)，即
noName(json); // callback(json);

// 所以这个匿名函数 noNane 其实就等于 callback
// 那么
return ajaxCall(function (json) {
    return callback(json);
});

// 就相等于
return ajaxCall(callback);

// 同样的道理
var getServerStuff = function (callback) {
    return ajaxCall(callback);
};

// 调用 getServerStuff 函数的时候
// 返回的是 ajaxCall(callback)，即
getServerStuff(callback); // ajaxCall(callback)

// 所以
var getServerStuff = ajaxCall;
```

另外，如果一个函数不必要的被包裹了起来，当它要发生改动的时候，那么包裹它那个函数也要做出相应的变化

```javascript
httpGet('/post/2', function (json) {
    return renderPost(json);
}
```

但突然发现 `httpGet` 要改成可以抛出一个可能出现的 `err` 异常，那这时我们就要把两个函数都改写

```javascript
httpGet('/post/2', function (json, err) {
    return renderPost(json, err);
}
```

但如果我们直接把 `renderPost` 函数当成参数传进去的时候，那就只要改 `renderPost` 函数就可以了

```javascript
httpGet('/post/2', renderPost);
```

## Pure Functions

> A	pure function is a function that, given the same input, will always return the same output and does not have any observable side effect.

所谓的纯函数就是对相同的输入，它总是能返回相同的输出，简单的说，就是执行了纯函数之后，不会对原来的数据有影响

再看一个例子

```javascript
//	impure
var minimum = 21;

var checkAge = function (age） {
    return age >= minimum;
};

// pure
var checkAge = function (age） {
    var minimum = 21;
    return age >= minimum;
};
```

在不纯的版本中，`checkAge` 的结果取决于 `minimum` 这个可变的变量，也就是说，它的值取决于系统的状态，当我们不知道在哪里修改这个变量的值的时候，`checkAge` 的返回值也会跟着改变

## What can *pure functions* do

- Cacheable

    一个纯函数总能根据输入来做缓存，这样不仅可以使同样的输入总是返回同样的东西，然后还可以提高运行的速率
    
    我们可以定义一个 `memoize` 函数，可以给它传入任意一个函数，不管这个函数有多大的破坏性
    
    ```javascript
    var memoize = function (fn) {
        var cache = {};
        
        return function () {
            var arg_str = JSON.stringify(arguments);
            cache[arg_str] = cache[arg_str] || fn.apple(fn, arguments);
            return cache[arg_str];
        };
    };
    
    // 定义一个 squareArea 函数
    var squareArea = memoize(function (x) { return x * x; });
    
    squareArea(5);  // 25
    squareArea(5);  // 25 : form cache
    ```
    
- Testable

    纯函数非常容易测试，你输入的是什么，输出就是什么，不会受环境的影响，所以不需要在测试前配置和模拟一堆的环境
    
- Parallel Code

    因为纯函数不会不会受外围环境还有变量的影响，所有不需要访问共享的内存，而且纯函数也不会因副作用而进入竞争态(race condition)
    
    代码并行在服务器端的 JavaScript 和使用了 [Web worker](#web-worker) 的浏览器是非常容易实现的，但对于现阶段来说，出于 Web worker 的兼容性还有非纯函数的复杂性来考虑，还是避免使用代码并行为好
    
## Curry

柯里化的概念很简单，只传递给一个函数一部分的参数来调用它，让它返回一个函数去处理剩下的参数

下面来看一个例子

```javascript
// 首先定义一个处理多参数的函数 
// 例如一个多参数求和 `add`

function add () {
    var sum = 0,
        i = 0;
    for (; i < arguments.length; i++) {
        sum += arguments[i];
    }
}

// 这时对 `add` 函数进行 柯里化
var currilyAdd = curry(add);

// 可以先处理一部分参数，来得到一个函数
var add10 = currilyAdd(10);

// 得到了一个 `add10` 函数，更直观，可读
console.log(add10(11)); // 21
```

那 `curry` 是怎么实现的呢

```javascript
function curry (fn) {
    // 返回 柯里化 后得到的函数
    return function () {
        // 会把第一次调用 柯里化后函数 的参数存起来
        var args = [].slice.apply(arguments);
        return function () {
            // 第二次调用的时候，把之前保存起来的参数
            // 一同调用原来的功能函数
            return fn.apply(fn, args.concat([].slice.apply(arguments)));
        };
    };
}
```

柯里化，运用的就是函数式编程的思想

它把已有的函数柯里化后，与参数结合得到一个新函数，使得这个新函数功能更加的清晰