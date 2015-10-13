# JavaScript: The Good Parts - Part 2

## Constructor

JavaScript 可以通过一个构造函数去产生我们的对象，然后用 `new` 关键字去生成我们对象的实例，当我们用 `new` 去执行我们的函数的时候，函数的执行方式就会修改，详情可以看这里 [This in Function](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/this-in-function.md)

但是用构造器函数存在一个严重的危害，如果你在调用构造器函数的时候忘记了在前面加上 `new` 前缀，那个 `this` 将不会被绑定到一个新的对象上， `this` 将被绑定到全局对象 `window` 上，所以你不但没有扩充新对象，反而破坏了全局变量环境。

## Inheritance

[Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/inheritance.md) 介绍了原型链继承，类式继承，组合式继承和原型继承

而在 JavaScript: The Good Parts 中还给我们提供了一种叫 `Functional` 的继承方式，它不仅仅可以实现继承，还可以在对象中设置私有属性和私有方法

### How "Functional" Works

1. 创建一个新对象
2. 定义私有属性和方法
3. 给这个对象扩充特权方法
4. 返回这个对象

例如我们定义一个 Animal 类，再定义一个 Cat 类来继承 Animal

```javascript
var Animal = function (name) {
    var that = {},
        name = name;
        
    var getName = function () {
        return name;
    };
    that.getName = getName;
    return that;
}
```

```javascript
var Cat = function (name, saying) {
    var name = name,
        saying = saying,
        that = Animal(name);
    
    var getSaying = function () {
        return saying;
    }
    that.getSaying = getSaying;
    return that;
}
```

```javascript
var cat = Cat('Jason', 'moew');
cat.getName();  // Jason
cat.getSaying();  // moew
```

为什么在为 `that` 定义特权方法的时候，要先定义私有方法，再将特权方法指向它，这样做有两个好处

- 当其他方法想要调用这个方法的时候，可以直接的调用，不需要加 `that.` 来调用
- 如果在实例中这个方法都替换掉了，但调用这个方法的其他方法还可以继续工作，因为它们直接调用的私有方法不受实例的影响

## Array

我们可以用 `[]` 来简单的定义我们的数组，那可以用简单的数字下标来调用我们的数组中的值，但事实上，JavaScript 中的数组都是对象

数组中的 `key` 是大于等于 0 的整数字符串，当然还是一个 "诡异" 的 `length` 属性

`length` 属性的值是由数组中最大的整数属性 + 1 来决定，它并不一定等于数组里的属性的个数

```javascript
var arrs = [];
console.log(arrs.length);   // 0

arrs[1000] = true;
console.log(arrs.length);   // 1001
```

而把 `length` 属性设小将会导致所有下标大于等于新 `length` 的属性被删除

```javascript
var arrs = ['one', 'two', 'three', 'four'];
arrs.length = 2;

console.log(arrs);  // ['one', 'two']
```

当我们数字下标去获取我们的值的时候，`[]` 会把它所含的表达式调用
`toString()` 方法，得到的字符串将被用作属性名，从而获得我们的值

因为数组其实是对象，所以我们可以用 `delete` 来删除我们的元素，但是删除的元素会变成 `undefined`

如果我们想移除我们的元素，我们可用 `splice` 方法，`splice` 的第一个参数是数组的一个序号，第二个参数是要移除多少个元素，其他的参数就会插入到数组中