# Classical Inheritance

> Prefer object composition to class inheritance.

类式继承其实有很多种的模式，现在我们就把每一种模式的实现和优缺点说一下

## 1.The Default Pattern

这一种称为默认模式其实我们常说的**原型链继承**，简单的来说，就是子类的原型是父类的实例

```javascript
function Person (name) {
    this.name = name;
}

Person.prototype.say = function () {
    console.log(this.name);
};

function  Child (name) {}

Child.prototype = new Person();
Child.prototype.constructor = Child;
```

他们为什么可以连在一起呢，靠的就是原型链，也就是 `__proto__` 属性。每个对象的 `__proto__` 都指向该对象的构造函数的 `prototype` 对象

默认模式的缺点就在于

- 一个子类继承了两个对象，一个是父类的实例，一个是父类的原型
- 构造子类的时候，没有办法给父类传递参数

## 2.Rent-a-Constructor

**借用构造函数模式** 可以解决从子构造函数到父构造函数的参数传递问题

```javascript
function Person (name) {
    this.name = name;
}

Person.prototype.say = function () {
    console.log(this.name);
};

function Child (name) {
    Parent.apply(this, arguments);
}
```

这样每当创建一个 `Child` 实例的时候，实例都会复制父类中的属性并将其作为自己的属性

我们还可以用这种方式来多重继承

```javascript
function Cat () {
    this.legs = 4;
    this.say = function () {
        return "meaowww";
    }
}

function Bird () {
    this.wings = 2;
    this.fly = true;
}

function CatWings () {
    Cat.apply(this);
    Bird.apply(this);
}

var jane = new CatWings();
console.log(jane);
// fly: true
// legs: 4
// wings: 2
// say: function ()
```

借用构造函数的缺点是 **不可以从原型中继承任何东西，就是子类不可以使用父类原型的方法**

其优点在于 **可以获得父类成员的副本，不会存在修改子对象会覆盖父对象属性的风险**

## 3.Rent and Set Prototype

这种模式就是我们常说的 **组合继承**

```javascript
function Child (name) {
    Person.apply(this, arguments);
}
Child.prototype = new Person();
```

这样做的优点是，即可以获得父对象本身的成员副本经及指向父对象原型，可以复用父类原型中的属性和方法外，还可以传递参数给父类的构造函数

这种模式的缺点在于父类的构造函数被调用了两次，第一次是在子类构造函数中，第二次是在定义子类原型的时候

## 4.Share the Prototype

**共享原型** 可以提供简短而迅速的原型链查询，这是由于所有的子类实际上都共享了同一个原型，但也会导致一个缺点，就是当我们修改了一个子类的原型，其他的类也会受到影响

```javascript
Child.prototype = Person.prototype;
```

## 5.A Temporary Constructor

**临时构造函数模式** 就可以解决共享原型的问题，修改子类的原型其实只是修改了上一层临时构造函数的实例

```javascript
function inherit (C, P) {
    var F = function () {};
    F.prototype = P.prototype;
    C.prototype = new F();
}
```

当然，我们还可以为我们的子类定义一个属性，来保存父类的原型，并且因为我们重新设置了子类的 `prototype` 对象，所以还应该重置 `constructor` 属性

```javascript
function inherir (C, P) {
    var F = function () {};
    F.prototype = P.prototype;
    C.prototype = new F();
    C.uber = P.prototype;
    C.prototype.constructor = C;
}
```

再对这个函数进一步的优化，就是仅创建一次临时构造函数，把它放在闭包里，重复使用

```javascript
var inherit = (function () {
    var F = function () {};
    return function (C, P) {
        F.prototype = P.prototype;
        C.prototype = new F();
        C.uber = P.prototype;
        C.prototype.constructor = C;
    }
}());
```