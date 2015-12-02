# Best Practices for Speeding Up Your Web Site

## Minimize HTTP Requests

减少 HTTP 的请求是提高你网页速度的关键

这里有一些减少 HTTP 请求的技巧

- **Combined files** - 把多个 JavaScript 文件或者多个 CSS 文件结合成一个文件，可以减少 HTTP 的请求
- **[CSS Sprites](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/css-sprites.md)** - 把你很多的前景图片结合成一张图片，然后用 `background-images` 和 `background-position` 来控制
- **Put Stylesheets at the Top** - CSS 应该放在 `head` 标签里
- **Put Script at the Bottom** - JS 应该放在 `</body>` 之前，但是并不是什么时候都可以，当该脚步需要用到 `document.write` 去把一些内容写到页面上的时候，我们就不可以把 JS 放到 `</body>` 之前。
- **Avoid CSS Expressions** - 
