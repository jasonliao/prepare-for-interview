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

## Inheritance with the prototype chain

原型链继承，简单的说就是子类的原型是父类的实例。

```javascript
var Person = function (age) {
  this.age = age;
};

Person.prototype = {
  sayAge: function () {
    console.log(this.age);
  }
};

var Student = function (name, age) {
  this.name = name;
  this.age = age;
};

Student.prototype = new Person();

var Jason = new Student('Jason', 21);
Jason.sayAge() // 21
```

这样就可以继承父类原型的方法。但是在构造子类的时候，没有办法给父类传递参数。

这时候就要用**类式继承(借用构造函数)**

## Classical inheritance

类式继承其实就是子类的构造函数用 `call` 借用了父类的构造函数。这样就可以给父类传递构造的参数了

```javascript
var Person = function (age) {
  this.age = age;
};

Person.prototype = {
  sayAge: function () {
    console.log(this.age);
  }
};

var Student = function (name, age) {
  Person.call(this, age);
  this.name = name;
};

var Jason = new Student('Jason', 21);
```

但是这样就还不可以与父类共享原型中的方法，所以一般我们会把**原型链继承**和**类式继承**组合使用，称**组合继承**

## Combination inheritance

```javascript
var Person = function (age) {
  this.age = age;
};

Person.prototype = {
  sayAge: function () {
    console.log(this.age);
  }
};

var Student = function (name, age) {
  Person.call(this, age);
  this.name = name;
};

Student.prototype = new Person();

var Jason = new Student('Jason', 21);
```

## [Prototypal inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md)

## Reference

- [Benefits of prototypal inheritance over classical?](http://stackoverflow.com/questions/2800964/benefits-of-prototypal-inheritance-over-classical)
- [Why Prototypal Inheritance Matters](http://aaditmshah.github.io/why-prototypal-inheritance-matters/)
- [Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

