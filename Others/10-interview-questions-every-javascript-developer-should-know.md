# 10 Interview Questions Every JavaScript Developer Should Know

在 Medium 上看到的[同名文章](https://medium.com/javascript-scene/10-interview-questions-every-javascript-developer-should-know-6fa6bdf5ad95#.xlsu9ejd7)，把知识点总结了一下，做成了笔记，把链接都贴在这

## 1. Can you name two programming paradigms important for JavaScript app developers?

JavaScript 是一种支持多种范式语言(multi-paradigm language,也就是说是多种编程方式，或者叫风格)，可以结合面向对象进行指令式／程序编程，还可以函数式编程。JavaScript 用 [原型继承](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md) 来实现面向对象编程

### Good to hear:

- [Prototypal inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md) (also: prototypes, OLOO).
- [Functional programming](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/functional-programming.md) (also: closures, [first class functions](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/functional-programming.md#first-class-functions), lambdas).

*lambdas: anonymous functions*

## 2. What is [functional programming](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/functional-programming.md)?

函数式编程是 JavaScript 中一个很重要的概念，越来越多的函数式编程的特征都被添加到 ES5 ES6 中

### Good to hear:

- [Pure functions](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/functional-programming.md#pure-functions) / function purity.
- Avoid side-effects.
- Simple function composition.
- Examples of functional languages: Lisp, ML, Haskell, Erlang, Clojure, Elm, F Sharp, OCaml, etc…
- Mention of features that support FP: [first-class functions](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/functional-programming.md#first-class-functions), higher order functions, functions as arguments/values.

*higher order functions: a function that takes or returns a function*

## 3. What is [the difference between classical inheritance and prototypal inheritance?](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#classical-inheritance-and-prototypal-inheritance)

### Good to hear:

- Classes: create tight coupling or hierarchies/taxonomies.
- Prototypes: mentions of [concatenative inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#concatenative-inheritance), [prototype delegation](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#prototype-delegation), [functional inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-The-Good-Parts/javascript-the-good-parts-part-2.md#how-functional-works), [object composition](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#inheriting-from-multiple-prototypes).

## 4. What are the pros and cons of [functional programming](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/functional-programming.md) vs object-oriented programming?

Get 唔到，睇原文

## 5. When is classical inheritance an appropriate choice?

### Good to hear:
- **NEVER**

[Classical Inheritance and Prototypal Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#classical-inheritance-and-prototypal-inheritance)

## 6.When is prototypal inheritance an appropriate choice?

> In JavaScript, prototypal inheritance is simpler & more flexible than classical inheritance.

[Prototypal Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md) 不仅仅有一种类型

- [**Delegation**](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#prototype-delegation) (i.e., the prototype chain)
- [**Concatenative**](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#concatenative-inheritance) (i.e. mixins, `Object.assign`)
- [**Functional**](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-The-Good-Parts/javascript-the-good-parts-part-2.md#how-functional-works) (Not to be confused with functional programming. A function used to create a closure for private state/encapsulation)

其实这三种类型都有最适合的应用场景，[Delegation](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#prototype-delegation) 一般是用来存放我们的方法，因为这些方法一般比较通用，不会常常修改。而 [Concatenative](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#concatenative-inheritance)  可以让我们结合多个原型的属性和方法，可以说成是多继承。最常见的就是我们的对象要继承我们的 `EventEmitter`，用来添加事件监听和事件驱动，例如 Flux 里的 Store。而 [Functional](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-The-Good-Parts/javascript-the-good-parts-part-2.md#how-functional-works) 则可以存储我们的私有变量和私有方法

## 7. What does “favor composition over class inheritance” mean?

Get 唔到，睇原文

## 8. What are two-way data binding and one-way data flow, and how are they different?

双向绑定是指页面的一些字段和数据动态的绑定在了一起，任意一端变化都会引起另一端的变化(Angular)。也因为这样，会有很多副作用，也不容易理解

数据单向流动是指数据模型才是王道，与页面的交互只能给数据模型信号，只有数据模型才可以去修改我们的数据，才可以去改变我们应用的状态(Flux)。因为数据是单向流动的，所以更容易理解

### Good to hear:

- React is the new canonical example of one-way data flow, so mentions of React are a good signal. Cycle.js is another popular implementation of uni-directional data flow.
- Angular is a popular framework which uses two-way binding.

## 9. What are the pros and cons of monolithic vs microservice architectures?

整体架构和微服务架构的好与坏...

Get 唔到，睇原文

## 10. What is asynchronous programming, and why is it important in JavaScript?

同步编程指编程的执行会受条件和功能调用的限制，代码总是从顶至底执行，当遇到网络请求和磁盘I / O的时候，就会阻塞

异步编程是指程序在 [Event Loop](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/event-loop.md) 里面跑，当我们有请求，有阻塞的时候，代码不会停在那里等结果，而是继续往下执行。当结果返回了，其他代码也执行完了的时候，就可以执行我们的回调函数，这样，单线程也能解决很多并发的问题

这在 JavaScript 中很重要是因为

- 页面本来就是异步的，他大部分的时间都在在等待着用户都他进行操作，然后再执行相应的回调
- 而在服务器端依旧如此，在我们等待请求返回的时候，还可以依旧接受其他的请求，这对服务器的性能非常有利

[What the hack is the event loop anyway](https://www.youtube.com/watch?v=8aGhZQkoFbQ) 对 Event Loop 理解，这个视频一定要看

### Good to hear:

- An understanding of what blocking means, and the performance implications.
- An understanding of event handling, and why its important for UI code.