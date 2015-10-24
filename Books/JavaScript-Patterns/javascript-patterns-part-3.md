# JavaScript Patterns - Part 3

## Namespace Pattern

命名空间模式是为了把全局变量减少到最少(最好只有一个)，新建一个全局变量，一个对象，之后就把所有的属性和方法都包在这个对象里，一般这个对象名用全部大写

```javascript
var MYAPP = {};
```

但是随着代码越来越多，很多变量不知道之前是否已经定义过，所以就要进行检验，不然就会导致变量的覆盖，同样的，在我们建立第一个全局变量，即我们的命名空间的时候，也要先检验是否已存在

```javascript
var MYAPP = MYAPP || {};
```

但如果每次在定义变量的时候，都这样检验，就会导致大量的重复代码，所以我们定义了一个 `namespace` 方法，去检验这个属性是否存在，存在则返回这个属性，不存在则创建一个并返回

```javascript
var MYAPP = MYAPP || {};

MYAPP.namespace = function (ns_string) {
    var parts = ns_string.split('.'),
        parent = 'MYAPP',
        i;
    
    if (parts[0] === 'MYAPP') {
        parts = parts.slice(1);
    }
    
    for (i = 0; i < parts.length; i++) {
        if (typeof parent[parts[i]] === 'undefined') {
            parent[parts[i]] = {};
        }
        parent = parent[parts[i]];
    }
    return parent;
};
```

命名空间模式的确可以解决很多命名的冲突，还可以解决和第三方代码的命名冲突，但同时，它也有一些缺点

- 定义变量的时候，要输入更多的字符，都要加上命名空间的前缀，代码量会增加
- 这个全局变量可以在任何部分代码里修改，并会影响到底下的功能
- 如果名字嵌套得很长，那么解析就要更长的时间

## Declaring Dependencies

声明依赖关系指的是在你的函数里，在最开头的地方把你依赖的对象或者模块定义为一个局部变量，这虽然很简单，但却有很多好处

- 显式的依赖声明可以向用你代码的人表明我需要什么依赖，你需要引入什么脚本文件
- 在函数顶部声明会让自己或者解析器更早的发现和解析
- 解析局部变量的速度比解析全局变量的速度要快，这会导致更好的性能
- 还有一些代码压缩的工具会把局部变量重命名为一个字符，这样就会使代码量更小

## Private Properties and Methods

### Private Members

当我们用构造函数来定义我们的类的时候，很容易就可以实现变量的私有性，只要我们用 `var` 声明一个变量，那么就不可以从外部访问和修改这个变量，所以我们还需要有访问这个变量的特权函数

```javascript
function Person () {
    var name = 'Jason'; // 私有变量
    this.getName = function () {
        return name;
    };  // 特权方法
}

var p = new Person();
console.log(p.name); // undefined
console.log(p.getName()); // 'Jason'
```

**注意，这种情况下我们还可以为 `p.name` 赋值，赋值之后，访问 `p.name` 的时候，就是我们赋值的结果，但是不会影响到 `p.getName()` 的结果**

```javascript
p.name = 'Liao';
console.log(p.name); // 'Liao'
console.log(p.getName()); // 'Jason'
```

### Privacy Failures

但是我们也有私有性失效的时候，就是当我们的私有变量是引用的时候，例如是一个对象，或者是一个数组，这时当我们修改我们的对象或数组引用的时候，我们的私有性就会失效

```javascript
function Person () {
    var name = {
        firstName: 'Jason',
        lastName: 'Liao'
    };
    this.getName = function () {
        return name;
    };  // 特权方法
}

var p = new Person();
var name = p.getName();
console.log(p.name); // undefined
console.log(name); // { firstName: 'Jason', lastName: 'Liao' }
name.firstName = 'Jobs';
console.log(name); // { firstName: 'Jobs', lastName: 'Liao' }
```

我们都知道，我们应该避免用构造函数去定义我们的类，不应该再使用 `new` 关键字，我们更应该直接用更简单，更容易理解的对象字面量的形式去创建我们的对象。

### Object Literals and Privacy

那用对象字面量的方式，我们怎么创建私有属性呢，这时我们就要用到闭包，创建一个 IIFE，返回一个对象，那 IIFE 里的变量就会变成私有变量

```javascript
var myobj = (function () {
    // 定义私有变量
    var name = 'Jason';
    
    // 返回一个对象，里面是特权方法
    return {
        getName: function () {
            return name;
        }
    };
}());

console.log(myobj.getName()); // 'Jason'
```

我们还可以用这种方法来为原型对象来定义私有变量，但刚刚说了，我们不建议有构造函数的方式来定义我们的对象

```javascript
// ... Person Constructor

Person.prototype = (function () {
    // 私有变量
    var address = 'Zhuhai';
    
    return {
        getAddress: function () {
            return address;
        }
    };
}());
```

## Module Pattern

模块模式是最常用的一种模式，它提供的结构化思想有助于组织日益增长的代码，这种模式包含了其他的几种模式

- 命名空间
- 即时函数
- 私有和特权成员
- 声明依赖

这种模式的结构如下

```javascript
// 命名空间模式，防止我们的变量冲突
MYAPP.namespace('MYAPP.util.array');

// 即时函数，保存私有属性，返回对象
MYAPP.util.array = (function () {
    
    // 依赖声明
    var uobj = MYAPP.util.object,
        ulang = MYAPP.util.lang,
        
    // 私有变量和方法
        array_string = '[object Array]',
        ops = Object.prototype.toString,
        isArray = function (arr) {
            return ops.call(arr) === array_string;
        };
        
    // 返回对象 包含特权方法
    return {
        isArray: isArray,
        // ... 更多属性和方法
    };
}());
```

### Importing Globals into a Module

我们有时候可能还会传入我们的全局变量，这样有助于加速即时函数中的全局变量的解析，因为传入的实参就会成为函数的局部变量

```javascript
MYAPP.util.array = (function (app, global) {
    
    var uobj = app.util.object,
        ulang = app.util.lang, 
        global = global;
        
    // ...
        
}(MYAPP, this));
```

## Chaining Pattern

链模式可以让我们一个接一个的调用对象的方法，而无需将前一个操作返回的值赋给变量，并且无需将你的调用分割成多行，我们熟悉的 jQuery 就是采用这种模式

```javascript
$('#main').css('display', 'block').css('background', 'red');
```

我们只要在定义方法的最后，添加 `return this` 就会简单地实现这一功能

### Pros and Cons of the Chaining Pattern

链模式的一个优点在于可以节省一些输入的字符，并且还可以创建更简洁的代码，还有一个优点就是它可以让你分割函数，不需要创建实现太多功能的函数，从长远看来，这提高代码的可维护性

但是它的缺点就在于，这种方式更加难以调试，你不知道究竟是在哪一个方法发生失效

但是当我们编写方法的时候，没有明显和有意义的返回的时候，都可以返回 `this` 