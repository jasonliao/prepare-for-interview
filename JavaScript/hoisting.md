# Hoisting

> Hoisting is the act of moving the declarations to the top of the function.

就是当变量或者函数不管在哪里声明的时候，都会被提升到该作用域的顶部来声明

这里有三个要注意的地方

1. **JavaScript没有块级作用域**

2. **函数声明会覆盖变量声明，但不会覆盖变量赋值**

3. **函数声明会提升，函数表达式不会被提升**

## JavaScript 没有块级作用域

```javascript
var name = 'World!';
(function () {
  if (typeof name === 'undefined') {
    var name = 'Jack!';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();
```

打印出来的是`Goodbye Jack!`，因为 JavaScript 没有块级作用域，所以 if 语句里的`var name = 'Jack!'`, name 变量的声明会被提升在 if 语句之外，变量声明没有被赋值所以为 undefined，进入 if 语句，name 被赋值为  Jack，所以打印出来的是 `Goodbye Jack!` 真正执行的代码如下：

```javascript
var name = 'World!';
(function () {
  var name;
  if (typeof name === 'undefined') {
    name = 'Jack!';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();
```

## 函数声明会覆盖变量声明，但不会覆盖变量赋值

这句话又怎么理解？看看下面两个例子

```javascript
function value () {
  return 1;
}
var value;
console.log(typeof value);  // function
```

```javascript
function value () {
  return 1;
}
var value = 1;
console.log(typeof value);  // number
```

当你声明的函数和你声明的变量(未赋值)同名时，函数的声明会覆盖变量的声明，这就是为什么第一个例子为`function`的原因。而第二个例子中，函数声明覆盖了变量声明，但因为value重新被赋值为1，所以value的类型则变为 number

看看下面这个例子

```javascript
function foo (a) {
  console.log(a);
  var a = 2;
  function a () {}
  consol.log(a);
}
foo(1);
```

会有什么结果呢？变量 a 声明提升，函数 a 声明提升，同名时函数覆盖变量，所以第一次打印出的是`function a() {}`。然后 a 被重新赋值为 2，所以第二次打印的是 2，棒棒哒~ 真正的执行顺序代码如下：

```javascript
function (a) {
  var function a () {}
  var a;
  console.log(a);
  a = 2;
  console.log(a);
}
foo(1);
```

**一旦进入函数上下文，在执行之前，会先执行以下步骤，把属性填充到该函数的作用域里**

- **函数的形参 - 属性名就是形参的名字，其值就是实参的值；对于没有传递的参数，其值为 undefined**
- **函数声明 - 属性名和属性值都是由函数创建出来的，如果作用域里有相同的属性名，则替换它的值**
- **变量声明 - 属性名就为变量名，其值为 undefined，如果变量名和已经声明的函数名和函数和参数名相同，则不会影响已经存在的属性**

# 函数声明会提升，函数表达式不会被提升

```javascript
function test () {
  foo(); // TypeError "foo is not a function"
  bar(); // "this will run!"
  var foo = function () {
    console.log("this won't run!");
  }
  function bar () {
    console.log('this will run!');
  }
}
test();
```

最后用一例

```javascript
var a = 1;
function b () {
  a = 10;
  return;
  function a () {}
}
b();
console.log(a); // What is your 'a'? :)
```

## Reference

- [JavaScript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)
- [Variable Object](https://github.com/goddyZhao/Translation/blob/master/JavaScript/%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1%EF%BC%88Variable%20object%EF%BC%89.md)

