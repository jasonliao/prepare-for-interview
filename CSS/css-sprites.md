# CSS Sprites

> CSS sprites technique is a way to reduce the number of HTTP requests made of image resources, by combining images in a single file.

## Why

- 可以把很多很小的图片放在一个文件了，这样就可以减小对服务器的请求，这样就可以缩短网页加载的时间。
- 通常来说，放在同一个文件后文件反而更小。
- 不需要用 JavaScript 去控制，只需要 CSS 就可以了

## How

只需要用 `background-position` 属性来控制就可以了
