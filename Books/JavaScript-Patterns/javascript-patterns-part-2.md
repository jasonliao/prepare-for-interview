# JavaScript Patterns - Part 2

## Global Variables

我们当然知道全局变量越少越好，但是我们常常会在不知不觉中创建了全局变量，那是因为 JavaScript 的两个特性

- JavaScript 可以无需声明，直接使用变量
- JavaScript 中未经声明的变量，就为全局对象所有

当你在函数中这个使用或者声明变量的时候，你就已经创建了全局变量

```javascript
function sum (x, y) {
    result  = x + y;
    return result;
}

function foo () {
    var a = b = 0;
}

// you got `result` and `b` as global variables
```

## Side Effects When Forgetting *var*

用 `var` 声明的全局变量和不用 `var` 声明的隐式的全局变量又有一点点的不同，声明的全局变量不能用 `delete` 删除，而隐式的全局变量却可以

```javascript
var global_var = 1;
globl_novat = 2;
(function () {
    global_fromfun = 3;
});

delete global_var; // false
delete global_novar; // true
delete global_fromfun; // true
```

## *for* Loops

在进行 `for` 循环的时候，尽量把遍历的数组或都类数组对象的 `length` 属性缓存起来，这样就不用每次循环的时候，都要访问一次数据的长度，这样会使代码变慢，特别是当数组是 HTML 容器对象的时候

- document.getElementsByName()
- document.getElementsByClassName()
- document.getElementsByTagName()
- document.images
- document.links
- document.forms
- document.forms[0].elements
- ...

之所以说当遍历 HTML 容器对象更慢是因为每次都是重新遍历一次 DOM Tree，而且通常 DOM 操作也是非常耗时的
