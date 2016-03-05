# Event Bubble & Event Delegation

## Event Bubble

DOM 事件流包括三个阶段，**事件捕获**、**处于目标阶段**和**事件冒泡阶段**

当你点击一个元素的时候，浏览器并不知道你所点击的确定元素是哪个，它会从 DOM tree 的最高层一层一层往下找，尽可能地找到这个元素所处的最低一层，这就是我们的捕获阶段

当浏览器找到这个元素的最低层的时候，就会触发绑定在这个元素上的 handler，这就是我们的第二个阶段，处于目标阶段

当执行完这个 handler 的时候，浏览器就会根据捕获时的路径，往回走，这时候就会触发绑定在父元素的 handler，这个阶段就是我们的事件冒泡阶段，正是因为有了冒泡，我们才有了强大的事件委托

## Event Delegation

Event delegation allows you to avoid adding event listeners to specific nodes; instead, the event listeners is added to one parent.

### Why

我们为什么要用到事件委托？

- 如果我们对每个特殊的元素节点进行监听，那我们就会有元素节点个监听，但如果我们使用了事件委托，我们只需要添加一个监听在这些元素节点的父元素上，根据冒泡事件来分析用户对哪一个特定的元素节点进行了操作，从而进行相应的处理。

- 如果你的元素节点会增加或者删除，那你也要对这些元素节点增加或者删除监听，这就会变得非常麻烦。

### How

如果我们把监听添加在了元素节点的父元素上，我们怎么知道我们点击的是哪一个呢？


```html
<ul id="parent-list">
  <li id="post-1">Item 1</li>
  <li id="post-2">Item 2</li>
  <li id="post-3">Item 3</li>
  <li id="post-4">Item 4</li>
  <li id="post-5">Item 5</li>
  <li id="post-6">Item 6</li>
</ul>
```

```javascript
document.getElementById('parent-list').addEventListener('click', function (e) {
  // e.target is the clicked element!
  if (e.target && e.target.nodeName == 'li') {
    console.log('List item', e.target.id.replace('post-'), 'was clicked!');
  }
});
```

我们可以通过事件对象的 `target` 属性来拿到我们真正点击的那个节点。

### Compatibility

IE9 之前，不兼容 `addEventListener`，但是有 `attachEvent`

```javascript
if (element.addEventListener) {
  element.addEventListener(type, fn, false);
} else if (element.attachEvent) {
  element.attachEvent('on' + type, fn);
} else {
  element['on' + type] = fn;
}
```

但是如果每次都进行判断，就会影响性能

所以我们可以用函数的惰性载入来提高性能，我们重新定义一个 `addEvent` 的函数。

- 这种方法会判断后为我们的 `addEvent` 方法重新定义，在最后执行并且返回。这样只会在第一次调用的时候损失一点性能。

```javascript
var addEvent = function (type, element, fn) {
  if (element.addEventListener) {
    addEvent = function (type, element, fn) {
      element.addEventListener(type, fn, false);
    };
  } else if (element.attachEvent) {
    addEvent = function (type, element, fn) {
      element.attachEvent('on' + type, fn);
    };
  } else {
    addEvent = function (type, element, fn) {
      element['on' + type] = fn;
    }
  }

  return addEvent(type, element, fn);
};
```

- 这种方法则是把一个自执行的函数赋给 `addEvent`，返回一个函数。这样就会在加载的时候损失一点性能。

```javascript
var addEvent = (function () {
  if (element.addEventListener) {
    return function (type, element, fn) {
      element.addEventListener(type, fn, false);
    };
  } else if (element.attachEvent) {
    return function (type, element, fn) {
      element.attachEvent('on' + type, fn);
    };
  } else {
    return function (type, element, fn) {
      element['on' + type] = fn;
    }
  }
})();
```

在回调函数里，可以这样兼容地获取我们的 `target`

```javascript
function (event) {
  event = event || window.event;
  var target = event.target || event.srcElement;
  // do something else with target
}
```
