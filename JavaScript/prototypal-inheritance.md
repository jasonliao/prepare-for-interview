# Prototypal Inheritance

在大部分的语言中，都是类和对象的概念，类继承于其他类

但是在 JavaScript 中，继承是基于原型的，这意味着我们没有真真切切的 "类"，我们是由对象继承于对象的(OLOO-Objects Linking to Other Object)

JavaScript 中有很多种的实现继承的方法，这篇主要是写 **原型继承**

## *\__pro__*

当一个对象继承于一个对象的时候，JavaScript 就会给子对象一个特殊的属性 `__proto__`，用于指向他的父对象

当访问一个子对象的属性和方法的时候，如果在子对象中找不到，就会沿着 `__proto__` 一直找到父对象

```javascript
var animal = { eat: true };
var cat = { died: false };

cat.__proto__ = animal;

console.log(cat.eat); // true
```

但是我们却不建议使用 `__proto__`，第一他有浏览器兼容的问题，第二个他还可能会带来性能的问题，[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto) 告诉我们，要使用 `Object.create` 和 `Object.getPrototypeOf` :)

## *Object.create* and *Object.getPrototypeOf*

```javascript
var animal = { eat: true };
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

## Prototype Delegation

正如上面说的，JavaScript 中的每一个对象都有一个 `__proto__` 属性，指向的就是这个对象的原型。`__proto__` 最终会指向 `null`，也就是我们常常说的原型链

而所谓的原型委托就是指当我们尝试着去访问一个对象的一个属性的时候，JavaScript 引擎会先到这个对象里找，如果找不到就会把这个属性**委托**到这个对象的原型上，一层层找不去，如果找到 `null` 都找不到，则返回 `undefined`

这个过程用代码表示就是这样

```javascript
function getProperty (object, property) {
    if (!object.hasOwnProperty(property)) {
        var prototype = Object.getPrototypeOf(object);
        if (prototype) {
            return getProperty(prototype, property);
        }
    } else {
        return object[property];
    }
}
```

## Concatenative Inheritance

但是我们刚刚说的其实都只是原型继承中的一种，叫 **委托继承**，或者叫 **差异性继承**，原型继承还有一个方式叫 **衔接继承**，或者叫 **克隆继承**

> 很多 JavaScript 程序员觉得这种方式不属于真正的继承，因为一个对象克隆了另一个对象的所有属性，那修改了原始对象的属性的时候，并不会影响到克隆对象

这两种原型继承的方法都有好与不好的地方

| Differential | Concatenative |
| :----- | :------ |
| 原型上的任何改变都会自动的影响到他的子类 | 原型上的改变要手动传播到子类中去 |
| 访问对象属性的时候要在原型链上搜索，所以可能会更慢 | 访问对象属性更快是因为属性都已复制到子类上 |
| 在 JavaScript 中，一个对象可能只继承一个原型对象 | 一个对象可能从多个原型对象中复制属性 |

## Inheriting from Multiple Prototypes

多继承就是原型继承比类式继承强大的地方

我们只要定义一个 `combine` 函数，就可以把多个对象中的属性克隆到我们的子对象中来

```javascript
Object.prototype.combine = function () {
    var object = Object.create(this),
        length = arguments.length,
        index = length;
        
    while (index) {
        var combineObject = arguments[length - (index--)];
        for (var property in combineObject) {
            if (combineObject.hasOwnproperty(property) && 
                    typeof object[property] === 'undefined') {
                object[property] = combineObject[property];
            }
        }
    }
    return object;
};
```

在 ES6 中，JavaScript 给我们提供了 `Object.assign` 了

## *Classical Inheritance* and *Prototypal Inheritance*

- **Classical Inheritance:** 类继承于类，实例一般通过构造函数 `new` 出来，并且不支持多继承
- **Prototypal Inheritance:** 对象直接继承于对象，实例一般通过工厂函数或 `Object.create()` 方法实例出来，可以由多个不同的对象组成，轻松地选择性继承

传统语言的类式继承是不可变的，你不可以在运行的时候更改或者添加方法，而且很复杂，有很多类似抽象类，最终类(不可变类)，接口等等的概念，而原型继承就会灵活很多，它既可变，也可以不可变，没有 `prototype` `call` `new` 等等，你只要有对象和扩展对象就可以了

> In JavaScript, prototypal inheritance is simpler & more flexible than classical inheritance.

## Stop Using the *new* Keyword

JavaScript 给我们提供了构造函数去定义一个类，并用 `new` 关键字去创建我们的类的实例，但是毕竟 JavaScript 没有类的概念，所以用 `new` 关键字去执行我们的函数会导致一些问题的产生

如果你在调用构造器函数的时候忘记了在前面加上 `new` 前缀，那个 `this` 将不会被绑定到一个新的对象上， `this` 将被绑定到全局对象 `window` 上，所以你不但没有扩充新对象，反而破坏了全局变量环境

