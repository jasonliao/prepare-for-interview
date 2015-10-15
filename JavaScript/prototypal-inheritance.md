# Prototypal Inheritance

在大部分的语言中，都是类和对象的概念，类继承于其他类

但是在 JavaScript 中，继承是基于原型的，这意味着我们没有真真切切的 "类"，我们是由对象继承于对象的(OLOO-Objects Linking to Other Object)

JavaScript 中有很多种的实现继承的方法，这篇主要是写 **原型继承**

## *\__pro__*

当一个对象继承于一个对象的时候，JavaScript 就会给子对象一个特殊的属性 `__proto__`，用于指向他的父对象

当访问一个子对象的属性和方法的时候，如果在子对象中找不到，就会沿着 `__proto__` 一直找到父对象

```javascript
var animal ＝ { eat: true };
var cat = { died: false };

cat.__proto__ = animal;

console.log(cat.eat); // true
```

但是我们却不建议使用 `__proto__`，第一他有浏览器兼容的问题，第二个他还可能会带来性能的问题，[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto) 告诉我们，要使用 `Object.create` 和 `Object.getPrototypeOf` :)

## *Object.create* and *Object.getPrototypeOf*

```javascript
var animal ＝ { eat: true };
var cat = Object.create(animal);

console.log(cat.eat); // true
console.log(cat.__prorto__ === animal); // true
console.log(Object.getPrototypeOf(cat) === animal); // true
```

## How *Obejct.create* works

```javascript
Object.create = function (superClass) {
    function F () {};
    F.prototype = superClass();
    return new F();
}
```
## Just try!

用 `Object.create` 方法就可以克隆一个已经存在的对象

```javascript
var rectangle = {
    area: function () {
        return this.width * this.height;
    }
};

var rect = Object.create(rectangle);
```

这样子，`rect` 对象就继承了 `area` 这个方法，我们还看到 `rectangle` 是用对象字面量来创建的对象，其实就相当于下面这样

```javascript
var rectangle = Object.create(Object.prototype);

rectangle.area = function () {
    return this.width * this.height;
};
```

这里我们只要再对我们的新对象进行扩展就可以了

```javascript
rect.width = 5;
rect.height = 7;
console.log(rect.area());
```

## Classical Inheritance and Prototypal Inheritance

传统语言的类式继承是不可变的，你不可以在运行的时候更改或者添加方法，而且很复杂，有很多类似抽象类，最终类(不可变类)，接口等等的概念，而原型继承就会灵活很多，它既可变，也可以不可变，没有 `prototype` `call` `new` 等等，你只要有对象和扩展对象就可以了

不是说类式继承不好，只是说类式继承不适合 JavaScript

## Stop Using the *new* Keyword

JavaScript 给我们提供了构造函数去定义一个类，并用 `new` 关键字去创建我们的类的实例，当我们用 new 去执行我们的函数的时候，函数的执行方式就会修改，详情可以看这里 [This in Function](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/this-in-function.md)

但是毕竟 JavaScript 没有类的概念，所以用 `new` 关键字去执行我们的函数会导致一些问题的产生

如果你在调用构造器函数的时候忘记了在前面加上 `new` 前缀，那个 `this` 将不会被绑定到一个新的对象上， `this` 将被绑定到全局对象 `window` 上，所以你不但没有扩充新对象，反而破坏了全局变量环境。