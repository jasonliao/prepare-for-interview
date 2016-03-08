# preventDefault vs stopPropagation vs stopImmediatePropagation

- **preventDefault**: 阻止默认事件的发生，但不会阻止往上冒泡
- **stopPropagation**: 阻止事件继续往上冒泡
- **stopImmediatePropagation**: 当一元素绑定多个事件时，调动后不会触发下一个

## Event.preventDefault

```html
<div class="container">
  <a href="#" class="item element">Click Me!</a>
</div>
```

例如我们有一个这样的 HTML，当我们点击 `Click Me!` 的时候，默认就会发生跳转，但我们可以用 `preventDefault` 来阻止该事件的发生
 
```javascript
$('item').on('click', function (e) {
  e.preventDefault(); 
  console.log('item was clicked'); // 只 `item was clicked`，不会跳转
});
```

## Event.stopPropagation

但如果我们在 `container` 也绑定了事件时，由于事件冒泡，绑定在 `container` 上的事件也会触发，像这样

```javascript
$('container').on('click', function (e) {
  console.log('container was clicked');
});

$('item').on('click', function (e) {
  e.preventDefault(); 
  console.log('item was clicked');
});
```

这时候，如果我们点击 `Click Me!` 时，同样的不会发生跳转，但是却不仅仅打印 `item was clicked`，还会打印 `container was clicked`。这类似于 [事件委托](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/event-bubble-and-event-delegation.md)。

如果我们不想事件继续往上传播，我们就可以使用 `stopPropagation`

```javascript
$('container').on('click', function (e) {
  console.log('container was clicked');
});

$('item').on('click', function (e) {
  e.preventDefault(); 
  e.stopPropagation();
  console.log('item was clicked'); // 只会打印 `item was clicked`
});
```

## Event.stopImmediatePropagation

像上面说了，如果一个元素绑定了多个事件，这个方法就可以阻止下一个方法的触发

```javascript
$('item').on('click', function (e) {
  e.preventDefault(); 
  console.log('item was clicked');
});

$('element').on('click', function (e) {
  console.log('element was clicked');
});
```

像上面这样，我们阻止了跳转的事件，但是因为我们的一个元素绑定了两个事件，我们会看到依次打印出 `item was clicked` 和 `element was clicked`

```javascript
$('item').on('click', function (e) {
  e.preventDefault(); 
  e.stopImmediatePropagation();
  console.log('item was clicked');
});

$('element').on('click', function (e) {
  console.log('element was clicked');
});
```

这样子，我们就只会看到 `item was clicked`

## return false

`return false` 提供了给我们一个更加快捷的方式，它相当于我们同时调用了 `preventDefault` 和 `stopPropagation`，但不会有 `stopImmediatePropagation` 的效果

```javascript
$('container').on('click', function (e) {
  console.log('container was clicked');
});

$('item').on('click', function (e) {
  console.log('item was clicked'); // 只会打印 `item was clicked`
  return false;
});
```