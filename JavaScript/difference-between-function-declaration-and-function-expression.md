# Difference Between Function Declaration and Function Expression

> FunctionDeclaration :
> function Identifier ( FormalParameterList opt ) { FunctionBody }

> FunctionExpression :
> function Identifier opt ( FormalParameterList opt ) { FunctionBody }

## Function Declaration

函数声明一般具有以下特性

- 有函数名
- 在进入上下文时创建出来
- 会影响变量对象
- 符合下列这种形式

  ```javascript
  function foo () {}
  ```

会影响变量对象，是因为通过函数声明的函数在进入上下文的时候就会被创建出来，即在执行代码阶段就已经存在，简单来说就是 [Hoisting](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/hoisting.md)。

## Function Expression

函数表达式一般具有以下特性

- 名字可有可无
- 在执行代码阶段创建出来
- 不会影响变量对象
- 必须在表达式的位置
- 最简单的形式

  ```javascript
  var foo = function () {}
  ```

正如上面的形式，我们把一个匿名函数赋给了变量 `foo`，之后就可以通过 `foo` 来访问函数了 - `foo()`

当然也可以有名字的，像这样

```javascript
var foo = function _foo () {}
```

当这样定义一个函数的时候，在外部只能通过 `foo` 来访问，但是在函数的内部，不仅仅可以用 `foo` 来访问，还可以用 `_foo` 来访问

当函数表达式有名字的时候，我们很容易和函数声明搞混。如果要区分清楚最有效方便的方法就是：函数表达式总是在表达式的位置。下面是一些容易搞混的例子

```javascript
// 在括号中 (grouping operator) 只可能是表达式
(function foo () {});

// 在数组初始化中，同样是表达式
[function bar () {}];

// 逗号操作符也只能跟表达式
1, function baz () {};
```

刚刚也说了，函数表达式不会影响变量对象，在执行它的时候，才会被创建，所以在定义前定义后，都没有办法访问

```javascript
foo() // "foo" is not defined

(function foo () {});

foo() // "foo" is not defined
```

### What Function Expression for?

1. 可以封装作用域来对上下文隐藏辅助数据

  ```javascript
  (function () {

    // 初始化作用域  

  })();
  ```

2. 在执行代码阶段在条件语句中创建函数表达式，这样也不会影响变量对象

  ```javascript
  var foo = 10;

  var bar = (foo % 2 == 0
    ? function () { alert(0); }
    : function () { alert(1); }
  );

  bar();  // 0
  ```

### Named Function Expression Feature

函数表达式不会影响上下文的变量对象，这就意味着不论是在定义前还是在定义后，都是不可以通过名字来进行调用的

但它却可以通过自己的名字进行递归调用

```javascript
(function foo (bar) {
  
  if (ba) {
    return;
  }

  foo(true); // 'foo' name is available

})();

foo(); // 'foo' is not defined
```

因为在执行代码的时候，当看到 NFE 之后，该函数在作用域链中就会多出一个特殊的对象，这个特殊对象中有一个唯一的属性，key 为这个 NFE 的名字，其值就是对 这个 NFE 的引用。当退出这个上下文的时候，这个特殊的对象就会被移除，所以只能在函数内部通过 NFE 的名字调用

## Immediately-invoked Function Expression

- (function () {})();
- (function () {}());

这两种都可以使得函数立即调用，两者的差别可以看[这里](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/immediately-invoked-function-expression.md)

## Reference

- [Functions](https://github.com/goddyZhao/Translation/blob/master/JavaScript/%E5%87%BD%E6%95%B0%EF%BC%88Functions%EF%BC%89.md)
