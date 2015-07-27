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

## Prototypal inheritance

直接上例子，例如有一个半径是 5 的圆

```javascript
var circle = {
  radius: 5
};

circle.area = function () {
  var radius = this.radius;
  return Math.PI * radius * radius;
}
```

这时如果我定义另一个半径为 10 的圆

```javascript
var circle2 = {
  radius: 10,
  area: circle.area
};
```

其实就是改变一下特定的值，其他的和 circle 一样。

换句话说，你把 circle 当成了你另一个对象的原型，再把特定的属性值赋给那个对象。所以，我们可以这样

```javascript
var circle2 = Object.create(circle);
circle2.radius = 10;
```

### How `Object.create()` works

```javascript
Object.create = function (superClass) {
  function F () {};
  F.prototype = superClass;
  return new F();
}
```

可以看到，`Object.create` 把你的父类当成你子类的原型，这就是原型继承。

所以我们在创建子类的时候，只要再加上子类特定的属性和方法就可以了。

当然我们可以把上面的代码放在一个方法里，这样就可以在创建子类的时候，完成配置

```javascript
var Circle = {
  radius: 5, // default value
  create: function (radius) {
    var circle = Object.create(Circle);
    circle.radius = radius;
    return circle;
  },
  area: function () {
    var radius = this.radius;
    return Math.PI * radius * radius;
  }
};

var circle2 = Circle.create(10);
```

最后用 Person 和 Student 来实现一下原型继承

```javascript
var Person = {
  create: function (age) {
    var person = Object.create(Person);
    person.age = age;
    return person;
  },
  sayAge: function () {
    console.log(this.age);
  }
};

// Person ---> Object.prototype ---> null

var Student = Object.create(Person);
Student.create = function (name, age) {
  var student = Person.create(age);
  student.name = name;
  return student;
};
Student.sayName =  function () {
  console.log(this.name);
};

// Student ---> Person ---> Object.prototype ---> null

var Jason = Student.crate('Jason', 21);

// Jason ---> Student ---> Person ---> Object.prototype ---> null

Jason.sayAge(); // 21
Jason.sayName(); // Jason
```

## Benefits fo prototypal inheritance

原型继承非常简单，没有 `prototype`, `call`, `new` 

你只需要通过字面量的方式创建一个对象，然后用`Object.create`来完成继承和创建实例

而且更容易理解。

## Reference

- [Benefits of prototypal inheritance over classical?](http://stackoverflow.com/questions/2800964/benefits-of-prototypal-inheritance-over-classical)
- [Why Prototypal Inheritance Matters](http://aaditmshah.github.io/why-prototypal-inheritance-matters/)
- [Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

