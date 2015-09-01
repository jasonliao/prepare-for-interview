# Immediately-invoked Function Expression

> Explain why the following doesn't work as an IIFE: `function foo () {}()`
> What needs to be changed to properly make it an IIFE

需要先介绍关于表达式语句的限制

表达式语句不能以左大括号 `{` 开始，因为这样一来就和代码块冲突了，也不能以 `function` 开始，因为这样一来又和函数声明冲突了。看下面这两种情况

```javascript
function () {
  // ...
}();

function foo () {
  // ...
}();
```

这两种情况，都会抛出错误，只是原因不同

第一种，因为是以 `function` 开头的，所以解析器会以为是函数声明，但是却因为没有名字，所以就会报错，因为函数声明一定要有函数名

第二种，已经有名字了，已经会正确的执行，但是还是会抛出语法错误 - 组操作符内缺少表达式。因为在这里 `()` 会被当成*组操作符*来处理，而非函数的调用的 `()`，这也解释了开头的第一个问题

## SO, How?

很简单，把一个函数变成一个函数表达式，而不是函数声明就可以了，两者的区别在[这里](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/difference-between-function-declaration-and-function-expression.md)

而创建表达式最简单的方法就是用组操作符，像这样

```javascript
(function foo (x) {
  alert(x);
})(1);
```

但并不是所有的函数调用都要有括号，当函数本来就在函数表达式的位置的时候，那么解析器就会把这个函数当作函数表达式来处理，那就可以直接用 `()` 来表示调用了，像下面这个例子

```javascript
var foo = {

  bar: function (x) {
    return x % 2 != 0 ? 'yes' : 'no';
  }(1)

};

alert(foo.bar); // 'yes'
```

> 如果要在函数创建后立即对函数进行调用，并且函数不在表达式的位置的时候，就要用括号把函数包起来，再用括号调用。这种情况，其实是手动将其转换成函数表达式

> 如果函数本来就在函数表达式的位置，那就可以直接使用括号来进行调用

来看两种合法的 IIFE

1. `(function () {})();`

  这种就是我们刚刚说的第一种情况，先用 `()` 把函数手动转成函数表达式，再用 `()` 进行调用

2. `(function () {}());`

  这种就是我们说的第二种情况，在最外面 `()` 里创建函数会被看成是函数表达式，所以可以直接用 `()` 调用

当然啦，除了使用括号的方式将函数转成函数表达式之外，还有其他的方式，像下面

```javascript
1, function () {
  // ...
}();

!function () {
  //...
}();

...
```

## Reference

- [Functions](https://github.com/goddyZhao/Translation/blob/master/JavaScript/%E5%87%BD%E6%95%B0%EF%BC%88Functions%EF%BC%89.md)
