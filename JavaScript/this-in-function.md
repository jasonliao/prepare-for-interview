# "THIS" is Functon

function 里的 `this` 在不同的时候，会有不同的表现，一般会有以下四种情况

- Invocation as a function
- Invocation as a method
- Invocation as a constructor
- Invocatuon with the `apply()` and `call()` method

## Invocation as a Function

这是最常见最普遍的一种，一般的形式是这样的

```javascript
function ninja () {}
ninja();

var samurai = function () {};
samurai();
```

但这样子调用的时候，函数的上下文，也就是 `this` 的指向就是全局，即 `window` 

## Invocation as a method

当函数是一个对象的属性时并且通用该属性调用这个函数的时候，就是这种情况，像这样

```javascript
var o = {};
o.whatever = function () {};
o.whatever();
``` 

这个时候，`this` 的指向就是对象 `o`

## Invocation as a constructor

当我们把一个函数当作构造器的时候调用的时候，我们用 `new` 这个关键字

```javascript
function Creep ()　{
      return this;
}
var creep = new Creep();
```

当我们想上面这个调用函数的时候，会有下面几个特殊的动作发生

- 一个新的空对象会被创建
`var o = {};`
- 这个对象会被传到这个构造器中的当作 `this`，所以这个对象就变成了此时构造器的上下文
- 当没有任何明确的返回值的时候，这个对象就会被返回

当然啦，如果我们 `var creep = Creep()` 这样调用也是可以的，这时候 `this`　指向的就是 `window`，这就相当于把 `window` 赋给 `creep`，所以意义并不大

## Invocation with the `apply()` and `call()` method

当我们想把函数的上下文换成我们任意的想要的时候，我们就可以用 `apply()` 和 `call()` 方法

没错，函数也是有方法的，就像其他对象一样

- `apply()` 方法接受两个参数，第一个是我们想要函数执行的上下文，第二个是执行这个函数的参数数组
- `call()` 方法可以接受任意多个参数，第一个和 `apply()` 的一样，而后面的任意个参数就是执行这个函数的参数

### How do we decide which to use?

回答这一类问题的一个高层次的答案就是，我们会用使用更能提高我们代码可读性的那一个方法。但更为实际的答案就是，我们会根据我们拿到的参数类型来决定我们使用哪个方法

如果我们拿到的参数是一串变量，`call()` 方法就可以让我们直接把他扔进去，如果我们拿到的参数已经是一个数组了，那很显然 `apply()` 会是更好的选择

## Summary

函数可以通过多种方式去调用，不同的方式决定了上下文的不同，也就是 `this` 的指向

- 当函数普通调用的时候，上下文就是全局变量 `window`
- 当函数当成方法调用的时候，上下文就是拥有这个方法的对象
- 当函数当成构造器成调用的时候，上下文就是刚刚分配的新对象
- 当通过 `apply()` 和 `call()` 调用的时候，上下文就是你传进去的第一个参数

## Reference

> Secrets of JavaScript Ninja
