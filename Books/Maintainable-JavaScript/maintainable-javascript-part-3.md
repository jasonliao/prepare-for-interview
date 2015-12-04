# Maintainable JavaScript - Part 3

## Errors

任何类型的对象都可以通过 `throw` 来抛出，但是最常见的是 `Error` 对象，它只接收一个字符串参数，就是错误的信息

```javascript
throw new Error('Something bad happened.');
```

### Error Types

ECMA-262 定义了 7 种错误对象类型

- Error *全部 errors 的基础类型，不会被浏览器引擎抛出*
- EvalError *当执行 `eval()` 有错误的时候*
- RangeError *当一个数字超出范围的时候，例如 `new Array(-20)`*
- ReferneceError *当引用不存在的时候，例如调用 `null` 的方法*
- SyntaxError *语法错误*
- TypeError *类型错误*
- URIError *当不合格式的 URI 字符串传入 `encodeURI`，`encodeURICompoent`，`decodeURI` 或 `decodeURIComponent`*

我们还可以创建自己的 Error

```javascript
function MyError (message) {
    this.message = message;
}

MyError.prototype = new Error();
```

## Don't Modify Objects You Don't Own

- 原生对象
- DOM
- BOM
- 类库的对象

不要对上面的对象进行下列操作

1. 不覆盖方法

    JavaScript 中覆盖一个已存在的方法难以置信的容易
    
    ```javascript
    document.getElementById = function () {
        return null;
    }
    ```
    
    没有任何方法可以阻止覆盖 DOM 方法，这会让 JavaScript 库和其他依赖该方法的代码都失效
    
2. 不新增方法

    只需要创建一个函数赋值给一个已存在的对象属性，就可以为一个对象新增一个方法，新增方法不好在于，会导致命名冲突，因为一个对象此刻没有某个方法，不代表它未来也没有

3. 不删除方法

    最简单的删除一个方法的方式就是给对应的名字赋值为 `null`
    
    ```javascript
    document.getElementById = null;
    ```
    
    如果方法是在对象的实例上定义，也可以使用 `delete` 操作符来删除
    
    ```javascript
    var person = {
        name: 'Jason'
    };
    delete person.name;
    console.log(person.name); // undefined
    ```
    
    但 `delete` 只能对实例的属性和方法起作用，如果在 `prototype` 的属性或方法上使用 `delete` 是不起作用的
    
    ```javascript
    delete document.getElementById; // still work
    ```
    
如果一种类型的对象已经做到了你想要的大多数工作，那么继承它，然后再新增一些功能就可以了，关于 JavaScript 的继承，可以看 [Prototypal Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md) 和 [Classical Inheritance](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/classical-inheritance.md)

ES5 引入几个方法来防止对对象的修改

- `Object.preventExtensions` *不可扩展对象*
    
    ```javascript
    var person = { name: 'Jason' };
    
    Object.preventExtensions(person);
    console.log(Object.isExtensible(person)); // false
    
    person.age = 20;
    console.log(person.age); // undefined
    ```

- `Object.seal` *密封对象*

    密封了的对象不可以扩展，所有的属性和方法都不可以删除
    
    ```javascript
    var person = { name: 'Jason' };
    Object.seal(person);
    
    person.age = 20;
    console.log(person.age); // undefined
    
    delete person.name;
    console.log(person.name); // 'Jason'
    ```

    ```javascript
    var person = { name: 'Jason' };
    
    console.log(Object.isExtensible(person)); // true
    console.log(Object.isSealed(person)); // false
    
    Object.seal(person);
    
    console.log(Object.isExtensible(person)); // false
    console.log(Object.isSealed(person)); // true
    ```
    
- `Object.freeze` *冻结对象*

    最严格的级别就是冻结对象，冻结的对象不可以扩展，又是密封的，并且对象的已存在的属性也不可以修改
    
    ```javascript
    var person = { name: 'Jason' };
    
    console.log(Object.isExtensible(person)); // true
    console.log(Object.isSealed(person)); // false
    console.log(Object.isFrozen(person)); // false
    
    Object.freeze(person);
    
    console.log(Object.isExtensible(person)); // false
    console.log(Object.isSealed(person)); // true
    console.log(Object.isFrozen(person)); // true
    ```
