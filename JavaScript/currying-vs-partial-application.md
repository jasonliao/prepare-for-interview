# Currying vs Partial Application

> Currying and partial application are two ways of transforming a function into another function with a generally smaller arity.

> 局部套用和局部应用是把一个函数转成另一个含有较少参数数量的函数的两种方式

## Implementation

首先我们看看他们分别的实现(对于两个参数的函数):

- Curry - `const curry = fn => x => y => fn(x, y);`
- Partial Application - `const partApply = (fn, x) => y => fn(x, y);`

## Difference

两者的区别:

- 局部套用 (Curry) 时只传入一个函数作为参数
- 局部套用 (Curry) 总是产生嵌套的只有一个参数的函数，转化后的函数在很大程度上和原来的函数相同
- 局部应用 (Partial Application) 时不仅仅传入一个函数，还传入这个函数所需要的部分参数值
- 局部应用 (Partial Application) 会产生一个任意参数个数的函数，转化后的函数不同于原来的函数，因为需要的参数少了

## Example

下面是一个简单的例子，假如我们有一个 `add()` 函数

```javascript
const add = (x, y) => x + y;
```

### Curry

```javascript
const addC = curry(add);

addC(3)(5); // 8
```

`add()` 需要两个参数，Curry 之后，如上面所说，我们产生的总是只有一个参数的函数，所以我们需要 `addC(3)(5)` 这样一个一个参数调用，之所以说 *转化后的函数在很大程度上和原来的函数相同* 是因为 `add()` 和 `addC()` 同样是需要两个参数之后才能得到结果。

### Partial Application

```javascript
const add3 = partApply(add, 3);

add3(5); // 8
```

我们在 Partial Application 的时候，我们会传入一个函数和这个函数所需要的部分参数值，如传入 `add` 和 `3`，这时返回的 `add3` 是一个函数，但是现在却和 `add()` 函数不一样了是因为它所需要的参数会比 `add()` 所需的参数少，少的个数取决于在 Partial Application 的时候，传入了多少个参数值

ES5 里有一个原生的方法可以帮我们实现我们的 Partial Application，就是我们的 `Function.prototype.bind()`，所以我们可以通过这个函数实现上述的 `partApply()`

```javascript
const add3 = add.bind(null, 3);

add3(5); // 8
```

## Implementation Again

现在我们已经了解了什么是 Curry，什么是 Partial Application 了，你是不是已经开始发现我们一开头的实现只是针对只有两个参数的函数，但如果我们要转化的函数不仅仅只有两个参数呢?

### Curry ES6 Implementation

```javascript
const curry = fn => {
  const curried = (...args) => args.length >= fn.length ?
    fn.call(this, ...args) :
    (...rest) => curried.call(this, ...args, ...rest);
  return curried;
}
```

### Partial Application ES6 Implementation

```javascript
const partApply = (fn, ...args) => (...args) => fn(...args, ...args);
```

## Connect

有趣的是，但我们把一个函数 Curry 之后，再加入部分函数调用之后，就会形成 Partial Application

```javascript
const addC = curry(add);
const add3 = addC(3);

// equal
const add3 = partApply(add, 3);
```

## Reference

- [Currying versus partial application (with JavaScript code)](http://www.2ality.com/2011/09/currying-vs-part-eval.html) - 2ality
- [Currying vs Partial Application](http://www.datchley.name/currying-vs-partial-application/) - Dave Atchley