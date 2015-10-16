# JavaScript: The Good Parts - Part 4

## Awful Parts

### Semicoion Insertion

JavaScript 有一个自动修复机制，它试图通过自动插入分号来修正有缺损的程序，但是我们却不能指望它，例如下面这种情况

```javascript
return 
{
    status: true
};
```

这时候程序返回的就是 `undefined`

### *typeof*

```javascript
console.log(typeof null);   // 'object'
console.log(typeof /a/);    // 'object'
console.log(typeof NaN);    // 'number'
```

### Phony Arrays

JavaScript 中没有真正的数组，所以如果想知道一个变量是不是数组不可以直接用 `typeof` 来判断，一般情况下可以使用检查变量的 `constructor` 属性

```javascript
if (myArrs && typeof myArrs === 'object' &&
        myArrs.constructor === Array) {
    // myArrs 是一个数组
}
```