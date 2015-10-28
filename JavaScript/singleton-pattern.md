# Singleton Pattern

单例模式的意思就是某个特殊的类只有一个实例，这意味着第二次创建这个类的实例的时候，返回的就是和上一个实例一样的对象

在 JavaScript 中没有类的概念，所以当我们用对象对面量的方式去创建一个对象的时候，这个对象就是一个单例，因为每个对象字面量创建的对象都不一样，即使他们的属性和方法都一样

```javascript
var obj = {
    myprop = 'my value'
};

var obj2 = {
    myprop = 'my value'
};

console.log(obj == obj2); // false
```

这里需要注意的是，当我们说到 "单例模式" 的时候，可能是指 [Module Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-Patterns/javascript-patterns-part-3.md#module-pattern)

## Using *new*

JavaScript 中没有类的概念，但我们却可以用构造函数和 `new` 来模拟，但我们用这种方法的时候，对于单例模式，我们希望的结果是每一次我们 `new` 出来的对象都是一样的

```javascript
var uni = new Universe();
var uni2 = new Universe();
console.log(uni === uni2); // true
```

想要达到这种效果，我们可以在第一次调用构造函数的时候把这个对象缓存起来，但再次调用这个构造函数的时候就直接把这个缓存的对象返回，这样就可以实现我们上述的效果了。对此我们可以有几种方式

- 你可以用一个全局变量来保存这个对象，但全局变量总是不安全的，会被任何人给重写，所以不建议也不展开说
- 你可以在构造函数里把这个对象缓存在一个静态属性里，如 `Universe.instance`，这种方法很简单但是有一个缺点就是这个静态属性是暴露的，可能会在外部被修改
- 你可以创建一个闭包，把 `instance` 属性放在外部函数里，这样就可以保持了 `instance` 的私有性

## Instance in a Static Property

```javascript
function Universe () {
    if (typeof Universe.instance === 'object') {
        return Universe.instance;
    }
    
    this.start_time = 0;
    this.bang = 'Big';
    
    Universe.instance = this;
}

// Testing
var uni = new Universe();
var uni2 = new Universe();
console.log(uni === uni2); // true
```

缺点刚刚也说了，就是 `Universe.instance` 是公有的，可以在外部被修改

## Instance in a Closure

另一个方式则是创建一个闭包去保护我们这个 `instance` 单例对象

```javascript
function Universe () {
    var instance = this;
    
    this.start_time = 0;
    this.bang = 'Big';
    
    Universe = function () {
        return instance;
    }
}
```

这种方式也可以通过我们上面的 Testing，这个构造函数在第一次执行完之后，就会重写这个构造函数，变成直接返回 `instance`，这种方式叫 **_self-defining functions pattern_**，也叫惰性加载函数，这种模式不好的地方在于他会丢失在原来函数上定义的属性

```javascript
Universe.prototype.nothing = true;
var uni = new Universe();

Universe.prototype.everything = true;
var uni2 = new Universe();

// Testing
console.log(uni.nothing); // true
console.log(uni2.nothing); // true
console.log(uni.everything); // undefined
console.log(uni.everything); // undefined

console.log(uni.constructor === Universe); // false
```

那是因为在第一次实例化后，`Universe` 被另一个匿名函数重写了，所以他们的 `prototype` 就不同了，也就访问不到后来定义的属性了

所以更好的解决方法就是定义一个 IIFE，让他返回一个构造函数，把 `instanec` 放在 IIFE 里，这样就实现 `instance` 的私有化和保存构造函数的属性

```javascript
var Universe = (function () {
    var instance;
    
    return function () {
        if (instance) return instance;
        
        this.start_time = 0;
        this.bang = 'Big';
        
        instance = this;
    };
}());
```

## *Module Pattern* and *Singleton Pattern*

Module Pattern 更注重的是私有的属性和方法，返回的是一个对象的字面量而不是一个构造函数，所以不需要用 `new` 去创建对象

而在这里的 Singleton Pattern 注重的更是 instance 的唯一性，返回的则是一个构造函数，需要用 `new` 再去创建这个单例

我们可以把他们结合起来，组合成一种更加完整，更加简单好用的单例模式

```javascript
var mySingleton = (function () {
    // 创建 `instance` 变量去保存我们的唯一实例
    var instance;
    
    // 我们要保存私有属性，所以要创建一个闭包
    function init () {
        // 私有变量和方法
        function privateMethod () {
            console.log( "I am private" );
        }
        var privateVariable = "Im also private";
            
        // 返回公共属性和方法
        return {
            publicMethod: function () {
                console.log( "The public can see me!" );
            },
            publicProperty: "I am also public"
        };
    }
    
    // 我们还要用一个方法来判断是否已经实例过
    return {
        getInstance: function () {
            if (!instance) {
                instance = init();
            }
            return instance;
        }
    };
    
}());
```

这样我们创建实例的时候，就可以不需要 `new`，而且还可以保存私有变量

```javascript
var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log(singleA === singleB); // true
```
