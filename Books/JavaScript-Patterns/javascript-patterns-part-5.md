# JavaScript Patterns - Part 5

## Separation of Concerns

关注分离指的是我们的内容(HTML)，表现(CSS)还有行为(JavaScript)这三部分尽可能的相互独立，体现了渐进增强的思想。而实际上，关注分离意味着

- 通过将 CSS 关闭来测试页面是否仍然可用，内容是否依然可读
- 将 JavaScript 关闭来测试页面是否仍然可以执行正常功能，所有的连接是否能够正常工作，所有表单是否可以正常工作并正确提交信息
- 不使用内联的处理器或内联样式属性
- 语义化的 HTML

## DOM Access

对 DOM 的获取是很影响性能的，我们应该对 DOM 的获取减少到最低

- 避免在循环中使用 DOM
- 将 DOM 的引用赋值给局部变量，然后使用这些局部变量
- 在可能的情况下使用 `querySelector` `querySelectorAll` (IE8+)
- 当 HTMLCollection 重复使用的时候，缓存它的长度

DOM 的获取有很多种的方法

- `getElementById()`
- `getElementsByClassName()` (IE9+)
- `getElementsByName()`
- `getElementsByTagName()`
- `querySelector()` (IE8+)
- `querySelectorAll()` (IE8+)

## DOM Manipulation

对 DOM 的操作会导致浏览器重新计算元素的尺寸和重绘，这会带来很大的性能问题，所以还是对 DOM 的操作减少到最低

### Create

`createElement()` 就是 JavaScript 给我们提供的创建节点的方法，想创建文本节点还可以用 `createTextNode()`

可 jQuery 则方便得多，我们只要创建字符串就可以了

### Insert

对于 DOM 的插入而言，我们可以先用 *document fragment* 来建立我们的子树，最后再使用一次的 `appendChild` 来把 *document fragment* 插入到 DOM 中

```javascript
var ul = document.getElementsByTagName("ul")[0],
    docfrag = document.createDocumentFragment(),
    browserList = [
        "Internet Explorer",
        "Mozilla Firefox",
        "Safari", "Chrome", "Opera"
    ];

browserList.forEach(function(e) {
    var li = document.createElement("li");
    li.textContent = e;
    docfrag.appendChild(li);
});

ul.appendChild(docfrag);
```

同样的，插入 DOM 节点还有 `insertBefore(newNode, referenceNode)` 方法

而在 jQuery 中，则提供了更加丰富的方法，插入我们有 `after()` `append()` 都是插入在节点后，而 `before()` 就是插入在节点前，`prepend()` 则是插入在节点里，即作为子节点插入

同时，jQuery 还提供了对应的 `insertAfter()` `appendTo()` `insertBefore()` `prependTo()` 等对应的方法，功能是一样的，**只是前者为调用者为已存在节点，参数为新增节点；而后者则是调用者为新增节点，参数为已存在节点**

### Remove

原生 JavaScript 给我们提供了 `removeChild(childNode)` 方法

而在 jQuery 中，有 `empty()` 把调用者的所有子节点清除掉，而 `remove()` 方法则是把调用者本身及其子节点一并清除掉

### Replace

原生 JavaScript 给我们提供了 `replaceChild(newChild, oldChild)` 方法让我们可以把父节点里的子节点替换掉

而在 jQuery 中，提供了 `replaceAll()` 和 `replaceWith()` 方法，前者调用者为新节点，参数为旧节点；则后者调用者为旧节点，参数为新节点

## Events

我们常常要为浏览器添加事件，但在 IE9 之前，我们要常常解决兼容性的问题，但我们要为一个按钮添加一个点击事件时，我们可以很简单的在 HTML 页面里

```html
<button id="clickme" onclick="handleClick">Click me: 0</button>
```

但是我们却要遵循渐进增强的原则，只把交互写在 JavaScript 里。所以我们可以为节点添加 `onclick` 事件

```javascript
var btn = document.querySelector('#clickme'),
    count = 0;

btn.onclick = function () {
    count += 1;
    btn.innerHTML = 'Click me: ' + count;
};
```

但是如果这样就有可能会覆盖掉这个按钮之前是的 `onclick` 操作，虽然你可以先判断有没有这个事件，如果有则把你的处理再另外加上去，但更好的办法则是使用 `addEventListener()`，但是在 IE9 之前，IE 只提供了 `attachEvent()` 方法

这时候我们可以自己定义一个 `addListener` 的方法，在里面做判断

```javascript
var addListener = function (event, target, handler) {
    if (document.addEventListener) {
        target.addEventListener(event, handler, false);
    } else if (document.attachEvent) {
        target.attachEvent('on' + event, handler);
    } else {
        target['on' + event] = handler;
    }
};
```

但如果我们每次执行这个函数都要判断的话，性能就会很差，这时我们可以使用惰性加载函数来解决这个问题，但也有两种方式

- 在第一次执行时作判断
    
    ```javascript
    var addListener = funtion (event, target, handler) {
        if (document.addEventListener) {
            addListener = function (event, target, handler) {
                target.addEventListener(event, handler, false);
            }
        } else if (document.attachEvent) {
            addListener = function (event, target, handler) {
                target.attachEvent('on' + event, handler);
            }
        } else {
            addListener = function (event, target, handler) {
                target['on' + event] = handler;
            }
        }
    };
    ```

- 在文件解析的时候作判断

    ```javascript
    var addListener = (funtion (event, target, handler) {
        if (document.addEventListener) {
            return function (event, target, handler) {
                target.addEventListener(event, handler, false);
            }
        } else if (document.attachEvent) {
            return function (event, target, handler) {
                target.attachEvent('on' + event, handler);
            }
        } else {
            return function (event, target, handler) {
                target['on' + event] = handler;
            }
        }
    }());
    ```

惰性加载函数还在[构造单例模式构造函数](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/singleton-pattern.md#instance-in-a-closure)的时候用到