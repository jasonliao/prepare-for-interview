# What's the Difference Between Standards Mode and Quirks Mode

## Why

因为在很久之前，页面一般会写成两个版本，一个是为了网景，另一个就是为了 IE。但后来 W3C 制定了标准，可是浏览器却不可以就这么直接用这些标准，因为如果这样做的话，很多旧的页面就不能用了。所以浏览器就会有两种不同的模式，去渲染符合标准的页面，和以前留下来的页面。

这两种不同的模式，就是

- Standards Mode(标准模式)
- Quirks Mode(怪异模式)

但现在浏览器的布局引擎一般会有三种模式

- Quirks Mode - 浏览器会用以前网景和 IE5 的非标准模式来渲染以前的网站
- Full Standards Mode - 以 W3C 制定的标准来渲染我们的 HTML 和 CSS
- Almost Standards Mode - 只有一小部分使用了怪异模式

## How do browsers determine which mode to use?

> For HTML documents, browsers use a DOCTYPE in the beginning of the document to decide whether to handle it in quirks mode or standards mode.

这也是 DOCTYPE 的作用 - 决定浏览器的呈现模式

为了确保你的页面用的是 Full Standards Mode，你应该使用 `<!DOCTYPE html>`，而且连 IE6 都支持，所以没有理由去使用其他更加复杂的 DOCTYPE 了。

> Make sure you put the DOCTYPE right the beginning of your HTML document. Anything before the DOCTYPE, like a comment or an XML declaration will trigger quirks mode in Internet Explorer 9 and older.

## What are the differences between the mode?

怪异模式下，浏览器会有以下行为，**但不是所有浏览器都会这样**

1. The **box model** is incorrect

  在怪异模式下，盒子的宽高与标准盒模型宽高不一样。这种模型也叫 IE 盒模型

  ```bash
  box width = content width + padding left + padding right + border left + border right;
  box height = content height + padding top + padding bottom + border top + border bottom;
  ```

  而 W3C 的标准盒模型

  ```bash
  box width = content width;
  box height = content height;
  ```

2. **Dimensions** of non-replaced inline elements

  `span` 就是 non-replaced 元素，在怪异模式下，定义这些元素的 width 和 height 属性，是可以影响该元素的尺寸大小的。

  - **Percentage height** of elements

  在怪异模式下，即使封闭块的 height 为默认值(auto)。元素的百分比高度也会起作用。但在标准模式下，元素的高度取决于内容的大小，只有在封闭块的 height 属性为特定值的时候，百分比高度才会起作用。

3. The **height of the `body` element** is 100%

  如果在标准模式下，就是在CSS里设置

  ```css
  html, body {
    height: 100%;
  }
  ```

4. **Overflow** is treated by expanding a box

  当我们的 `overflow: visible`(默认) 的时候，字会超过这个盒子显示。但是在怪异的模式下，盒子的大小就会被改变，如果盒子有背景色或者边框的时候，就会很容易被发现。

5. **Alt text** is not always shown

  怪异模式下，当图片不显示，并且 `img` 的尺寸小于 `alt` 属性的文本显示大小的时候，就会不显示 `alt` 的值

6. The **root element** is the `body` element on IE in quirks mode

7. A gray 2px **page border** appears by default on some versions of IE

8. **Padding for an image** is ignored when set in CSS for an `img` element or an `<input type="image">`

9. The declaration **white-space: pre** is ignored

10. and more in [What heppens in Quirks Mode?](https://www.cs.tut.fi/~jkorpela/quirks-mode.html)
