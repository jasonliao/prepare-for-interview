# Inheritance

先通过一张图来了解一下 `prototype` 和 `__proto__`

![javascript-object-layout](assets/javascript-object-layout.png)

- 所有东西都有 `__proto__` 这个属性(除了 `null`)，指向的就是他们类的原型，例如

  - 所有的 `function` 的 `__proto__` 都是指向 `Function.prototype`，包括普通的 `function Foo()` 还有 `function Object()` 和 `function Funtion()`
  - f1, f2 是 `Foo` 的实例，所以 `__proto__` 指向的是 `Foo.prototype`
  - 而 `Function.prototype` 的 `__proto__` 则是指向 `Object.prototype`
  - `Object.prototype` 的 `__proto__` 指向的是 `null`
  
- 只有 `function` 才有 `prototype` 属性，指向的是自己的原型，就像
  
  - `function Foo()` 的 `prototype` 就是指向 `Foo.prototype`
  - `function Object()` 的 `prototype` 就是指向 `Object.prototype`
  - `function Function()` 的 `prototype` 就是指向 `Function.prototype`

## [Classical Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/classical-inheritance.md)

## [Prototypal Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md)

## [Functional Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-The-Good-Parts/javascript-the-good-parts-part-2.md#inheritance)

## Reference

- [Benefits of prototypal inheritance over classical?](http://stackoverflow.com/questions/2800964/benefits-of-prototypal-inheritance-over-classical)
- [Why Prototypal Inheritance Matters](http://aaditmshah.github.io/why-prototypal-inheritance-matters/)
- [Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

