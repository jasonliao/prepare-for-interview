# Maintainable JavaScript - Part 2

## Detecting Primitive Values

原始数据类型有 5 种

1. 字符串
2. 数字
3. 布尔值
4. undefined
5. null

前四种我们最好的方法就是用 `typeof` 来检测，因为他始终返回这个值的类型的字符串，如 `string`，`number`，`boolean` 和 `undefined`

但是却不可以用 `typeof` 来检测 `null`，因为返回的却是 `object`

## Detecting Reference Values

引用类型都是对象，在 JavaScript 中除了原始值之外的值都是引用，例如内置的 `Object`，`Array`，`Date`，`Error`，检测这些引用类型就不可以上面的方式了，因为他们通通都会返回 `object`

检测引用类型最好的方法是使用 `instanceof`，`instanceof` 返回的 `true` 或 `false` 是检测该对象的构造器来判断的，但不仅如此，它还会检测原型链。例如每个对象都默认继承了 `Object`，所以所有的引用对象 `instanceof Object` 都会返回 `true`

当检测自定义类型的时候，`instanceof` 是唯一的方法

```javascript
function Person (name) {
    this.name = name;
}

var jason = new Person('Jason');

console.log(jason instanceof Person);   // true
```

## Detecting Functions

函数也是属于引用类型，所以我们也可以使用 `instanceof` 来检测

```javascript
function foo () {}

// Bad
console.log(foo instanceof Function);   // true
```

为什么不好是因为这个方法不可以跨帧使用，因为每一个帧都有各自的 `Function` 构造函数，但我们有更好的方法就是 `typeof`

```javascript
console.log(typeof foo);    // 'function'
```

但 IE8 及其更早的版本对 DOM 节点操作的函数返回的是 `'object'`

```javascript
console.log(typeof document.getElementById);  // 'object'
console.log(typeof document.createElement);  // 'object'
console.log(typeof document.getElementsByTagName);  // 'object'
```

## Detecting Arrays

```javascript
function isArray (value) {
    return Object.prototype.toString.call(value) === '[object Array]';
}
```

## Detecting Properties

检测属性值和检测属性有很大的区别，如果检测的是属性值，那么就可以用我们刚刚上面的检测方法，那如果是检测属性值是否存在的话，最好的方法就是用 `in`

如果你只想检查实例对象的某个属性是否存在的话，则使用 `hasOwnProperty()`，但需要注意的是　IE8 及更早的版本，DOM 对象及非继承于 `Object`，所以也不包含这个方法，所以如果你不知道这个对象是不是 DOM 对象，最好使用 `in`