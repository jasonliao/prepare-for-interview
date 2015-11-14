# Maintainable JavaScript - Part 1

> When you come to work, you’re not writing code for you, you’re writing code for those who come after you.

## null

在下列场景中应当使用 `null`

- 用来初始化一个变量，这个变量可能赋值为一个对象
- 用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象
- 当函数的参数期望是对象时，用作参数传入
- 当函数的返回值期望是对象时，用作返回值传出

在下列场景中不应当使用 `null`

- 不要使用 `null` 检测是否传入了某个参数
- 不要用 `null` 来检测一个未初始化的变量

理解 `null` 最好的方式就是将它当做对象的占位符

## undefined

```javascript
console.log(null == undefined); // true
console.log(null === undefined); // false
```

一般避免直接在代码中使用 `undefined`，因为这个值会与 `typeof` 返回的 `'undefined'` 混淆，事实上，`typeof` 的行为也很奇怪

```javascript
var person;
// foo 未声明
console.log(typeof person); // 'undefined'
console.log(typeof foo); // 'undefined'

console.log(typeof null); // 'object'
```

如果你使用一个可能赋值为对象的变量时，将其赋值为 `null`，这个使用 `typeof` 对 `null` 运算的时候，返回的就是 `'object'`

## Loose Coupling of UI Layers

### Keep JavaScript Out of CSS

停止使用 CSS 表达式 `expression`

### Keep CSS Out of JavaScript

我们可以使用以下的 JavaScript 代码来修改我们元素的样式

```javascript
element.style.color = 'red';
element.style.left = '10px';
```

但是我们更好的方法就是去操作元素的类，然后把对应类的样式写在 CSS 文件里

```javascript
// 原生方法
element.className += ' reveal';

// HTML5
element.classList.add('reveal');
```

### Keep JavaScript Out of HTML

一般我们会在 HTML 添加我们的响应事件

```html
<button onclick="dosomething()" >Click me</button>
```

但是这样就会把 HTML 和 JavaScript 造成了很大的耦合，所以我们会先在 JavaScript 里拿到我们要操控的元素之后，再给他绑定事件，绑定事件要考虑的兼容性问题在这里就不讨论了，详情可以看 [这里](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-Patterns/javascript-patterns-part-5.md#events)

### Keep HTML Out of JavaScript

在 JavaScript 中使用 HTML 的情形往往是给 `innerHTML` 属性赋值的时候

```javascript
var div = document.getElementById('my-div');
div.innerHTML = '<h3>Error</h3><p>Invalid e-mail address</p>';
```

这样做增加了跟踪文本的结构性问题的复杂度，增加了调试的困难，而且，在　JavaScript 里添加字符串要对一定的符号进行转义，会导致一定的错误和麻烦，要把 HTML 从 JavaScript 中抽离，有多种方法

- 从服务器加载
    
    把 HTML 的字符串放在服务器端，有需要的时候，再去加载，获取字符串

- 简单客户端模板

    在 HTML 里预先把要改变的内容用 `%s` 占位符，在这个位置的文本会被程序替换掉
    
    ```html
    <ul>
        <!-- <li><a href="%s">%s</a></li> -->
    </ul>
    ```
    
    例如这样我们就可以获得 `li` 的 HTML 字符串，并把 `%s` 替换成我们需要的东西，添加到 `ul` 里即可

- 复杂客户端模板

    我们可以直接使用第三方的模板，像 Handlebars、ejs、jade 等等
    
## Event Handling

### Classic Usage

例如我们要完成一个功能就是在特定的位置显示一个弹出框，我们会先定义一个函数，然后再绑定一个元素

```javascript
function handleClick (event) {
    var popup = document.getElementById('popup');
    popup.style.left = event.clientX + 'px';
    popup.style.top = event.clientY + 'px';
    popup.className = 'reveal';
}

addListener(element, 'click', handleClick);
```

但是这种写法，却有其局限性

### Separate Application Logic

上面实例的代码把用户行为和应用逻辑合在了一起，就是当用户点击的时候，显示弹出框，但是我们有时候不仅仅是用户点击的时候，才显示弹出框，我们有可能当用户把鼠标移到某个元素上的时候， 我们也要显示弹出框，所以我们应该把应用逻辑提取出来

这样对我们的测试也有好处，如果我们的应用逻辑和我们的事件处理放在一起的时候， 我们的唯一的测试方法就是去触发事件，则调用功能性代码最好的测试方法就是直接调用单个函数

所以我们可以把事件处理和应用逻辑分成两个不同的函数，然后放在一个对象中

```javascript
var MyApplication = {
    handleClick: function (event) {
        this.showPopup(event);
    },
    showPopup: function (event) {
        var popup = document.getElementById('popup');
        popup.style.left = event.clientX + 'px';
        popup.style.top = event.clientY + 'px';
        popup.className = 'reveal';
    }
};
```

### Don’t Pass the Event Object Around

我们不需要把我们的 `event` 对象一直传下去，因为我们的应用逻辑只在乎在哪里显示弹出框，所以我们可以直接给 `showPopup` 传入 `x`，`y`

```javascript
var MyApplication = {
    handleClick: function (event) {
        this.showPopup(event.clientX, event.clientY);
    },
    showPopup: function (x, y) {
        var popup = document.getElementById('popup');
        popup.style.left = x + 'px';
        popup.style.top = y + 'px';
        popup.className = 'reveal';
    }
}
```

如果我们还需要阻止默认事件或阻止事件冒泡，我们就可以在事件处理的函数里写，就会更加清楚地展示了事件处理和应用逻辑之间的分工

```javascript
var MyApplication = {
    handleClick: function (event) {
        event.preventDefault();
        event.stopPropagation();
        this.showPopup(event.clientX, event.clientY);
    },
    showPopup: function (x, y) {
        var popup = document.getElementById('popup');
        popup.style.left = x + 'px';
        popup.style.top = y + 'px';
        popup.className = 'reveal';
    }
}
```