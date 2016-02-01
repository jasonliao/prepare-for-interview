# What is a Closure?

> Closures are **frequently used in JavaScript** for object data privacy, in event handlers and callback functions, and in partial applications, currying, and other functional programming patterns.

> 闭包常常在 JavaScript 用作创建或保存数据的私有性，事件处理，和回调函数，当然还有局部调用，局部套用以及其他函数式编程模式中

## What is a Closure?

在 JavaScript 中，闭包会在每个函数在创建的时候被创建，在实际运用中，闭包就是可以给我们在内部函数访问外部函数作用域的一种机制

## How to Use

使用闭包的方法也很简单，只要在一个函数里面再创建一个函数，并把这个函数暴露出去就可以了，可以通过返回的方式，也可以通过赋值的方式

那么被暴露的那个函数就可以访问外部函数的作用域，即使当外部函数已经被返回了

## Using Closures (Examples)

### objects data privacy

- 在使用函数作为构造器创建对象的时候，我们可以用 `var` 在构造器里声明我们的私有变量

    ```javascript
    function Person () {
      var name = 'Jason';
      this.getName = function () {
        return name;
      }
    }
    
    var p = new Person();
    console.log(p.name); // undefined
    console.log(p.getName()); // Jason
    ```
    
    我们可以看到 `function () { return name; }` 这个匿名函数就是定义在 `Person` 函数里的一个同部函数，所以可以读取到外部函数的 `name` 变量

- 当我们使用对象字面量创建对象的时候，也需要用到我们不仅仅用到我们的闭包，还要用到 [IIFE](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/immediately-invoked-function-expression.md)

    ```javascript
    var p = (function () {
      var name = 'Jason';
      return {
        getName: function () {
          return name;
        }
      };
    }());
    
    console.log(p.name); // undefined
    console.log(p.getName()); // Jason
    ```
    
    我们 [Module Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-Patterns/javascript-patterns-part-3.md#module-pattern) 用的就是这一个方法
    
### partial application & currying

闭包在函数式编程中也起到了很重要的作用，常常使用在 [partial application & currying](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/currying-vs-partial-application.md) 中