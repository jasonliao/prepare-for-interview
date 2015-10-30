# JavaScript Patterns - Part 1

## Property Type

属性类型包括两种，一种是数据属性，另一种是访问器属性

### Data Properties

一个数据属性有四个描述其行为的特性

- **[[Configurable]]**: 决定了能否通过 `delete` 来删除属性，通过字面量创建的对象属性，这个默认值为 `true`
- **[[Enumerable]]**: 决定了 `for in` 循环能否找到这个属性，通过字面量创建的对象属性，这个默认值为 `true`
- **[[Writable]]**: 表示能否修改属性的值，通过字面量创建的对象属性，这个默认值为 `true`
- **[[Value]]**: 这个就是属性的值，默认为 `undefined`

但是在 JavaScript 中，不能直接访问他们，但在 ES5 中，JavaScript 给我们提供了一个 `Object.defineProperty` 这个方法，下面是怎么使用他的例子

```javascript
var person = {};
Object.defineProperty(person, 'name', {
    writable: false,
    value: 'Jason'
});

console.log(person.name);   // Jason
person.name = 'Liao';
console.log(person.name);   // Jason
```

我们把 `writable` 定义成了 `false`，这时如果我们想为这个属性重新赋值的时候，在非严格模式下，赋值操作就会忽略，严格模式下，就会抛出错误

同样的，我们还可以用这个方法来修改 `configurable` 属性，但这个属性一旦设置为 `false`，就再也不能用 `Object.defineProperty` 这个方法来修改除 `writable` 之外的特性了

在调用 `Object.degineProperty` 定义属性的时候，`configurable` `enumerable` `writable` 默认都是 `false`

### Accessor Properties

访问器属性也有四个特性

- **[[Configurable]]**: 决定了能否通过 `delete` 来删除属性，通过字面量创建的对象属性，这个默认值为 `true`
- **[[Enumerable]]**: 决定了 `for in` 循环能否找到这个属性，通过字面量创建的对象属性，这个默认值为 `true`
- **[[Get]]**: 读取属性时调用的函数，默认值为 `undefined`
- **[[Set]]**: 写入属性时调用的函数，默认值为 `undefined`

同样的，我们也可以通过 `Object.defineProperty` 来定义访问器属性

```javascript
var book = {
    _year = 2004,
    edition: 1
};

Object.defineProperty(book, 'year', {
    get: function () {
        return this._year;
    },
    set: function (value) {
        if (value > 2004) {
            this._year = value;
            this.edition += value - 2004;
        }
    }
});
```

这是使用访问器属性的常见的方式，设置一个属性的值会导致其他属性发生变化

在　`Object.defineProperty`　之前，浏览器也有对这 `get` 和 `set` 两个访问器属性的创建方法 - `__defineGetter__`， `__defineSetter__`

那上面的例子就可以重写这样

```javascript
book.__defineGetter__('year', function () {
    return this._year;
});

book.__defineSetter__('year', function (value) {
    if (value > 2004) {
        this._year = value;
        this.edition += value - 2004;
    }
});
```

### *Object.defineProperties*

我们可以使用 `Object.defineProperties` 来定义多个属性，不管是数据属性还是访问器属性

```javascript
var book = {};

Object.defineProperties(book, {
    _year: {
        value: 2004
    },
    edition: {
        value: 1
    }
    year: {
        get: function () {
            return this._year;
        },
        set: function (value) {
            if (value > 2004) {
                this._year = value;
                this.edition += value - 2004;
            }
        }
    }
});
```

### *Object.getOwnPropertyDescriptor*

这个方法可以取得给定属性的特性

```javascirpt
// 若是数据属性 则返回一个对象包括下列属性
// value writable enumerable configurable 
var D_year = Object.getOwnPropertyDescriptor(book, '_year');
console.log(D_year);
/* {
        value: 2004, 
        writable: false, 
        enumerable: false, 
        configurable: false
    }
*/

// 若是访问器属性 则返回一个对象包括下列属性
// get set enumerable configurable 
var Dyear = Object.getOwnPropertyDescriptor(book, 'year');
console.log(Dyear);

/* {
        get: function get (), 
        set: function set (value), 
        enumerable: false, 
        configurable: false
    }
*/
```

## *DefineProp*

我们可以看到，当我们用 `defineProperty` 或 `defineProperties` 定义属性的时候， `writable` `enumerable` `configurable` 这些属性都默认为 `false`，这样子之后想对属性操作都会被禁止，为了不每次定义属性都修改它们的默认值，我们可以自己定义一个 `defineProp` 的方法

```javascript
function defineProp (obj, key, value) {
    var config = {
        value: value,
        writable: true, 
        enumerable: true, 
        configurable: true
    };
    Object.defineProperty(obj, key, config);
}
```