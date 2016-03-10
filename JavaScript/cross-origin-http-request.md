# Cross-Origin HTTP Request

跨域并非浏览器限制了发起跨站的请求，而是跨站请求可以正常发起，但是返回结果被浏览器拦截了。请求始终发送到了后端服务器无论是否跨域。但是有一个特例，就是当 HTTPS 的跨域访问 HTTP 的时候，这个请求在还没有发出的时候已经被浏览器拦截

## 1. JSONP

JSONP 的原理是借助 `<script>` 标签的 `src` 属性可以实现跨域请求，只要把我们的 `callback` 在请求的时候一并传过去，那么其他域的数据就可以根据我们的定义的 `callback` 来使用。更多详情可以查看 [How JSONP Works](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/how-jsonp-works.md)

## 2. CORS

CORS(Cross-Origin Resource Sharing)，即跨域资源共享，他的原理是使用自定义的 HTTP 头部，主要是通过设置响应头的 `Access-Control-Allow-Origin` 来达到目的，这样 `XMLHttpRequest` 就可以跨域了

## 3. window.name

`window.name` 是一个当浏览器页面打开就存在的变量，值为 `''`。它的生命周期是一直维持到页面关闭，并且是共享的，所以我们可以用来传输一些数据

## 4. document.domain

在不同的子域和 `iframe` 交互的时候，获取到另外一个 `iframe` 的 `window` 对象是没有问题的，但是获取到的这个 `window` 的方法和属性大多数都是不能使用的。

这时就可以设置 `document.domain` 来解决

但是为什么是子域呢，那是因为 `document.domain` 的设置也是有限制的，它只能设置为页面本身或者更高一级的域名

## 5. location.hash

这种方法靠的是把数据的变化放在 `url` 的 `hash` 里。

## 6. window.postMessage

`window.postMessage()` 是 HTML5 新的 API，它允许和所有的域进行交流

## Reference

- [浅谈浏览器端JavaScript跨域解决方法](https://github.com/rccoder/blog/issues/5)