# Best Practices for Speeding Up Your Web Site

[YAHOO! 很经典的性能优化 35 计](https://developer.yahoo.com/performance/rules.html)

Table of Content

- [Minimize HTTP Requests](#minimize-http-requests)
- [Use a Content Delivery Network](#use-a-content-delivery-network)
- [Add an Expires or a Cache-Control Header](#add-an-expires-or-a-cache-control-header)
- [Gzip Components](#gzip-components)
- [Put Stylesheets at the Top](#put-stylesheets-at-the-top)
- [Put Scripts at the Bottom](#put-scripts-at-the-bottom)
- [Avoid CSS Expressions](#avoid-css-expressions)
- [Make JavaScript and CSS External](#make-javascript-and-css-external)
- [Reduce DNS Lookups](#reduce-dns-lookups)
- [Minify JavaScript and CSS](#minify-javascript-and-css)
- [Avoid Redirects](#avoid-redirects)
- [Remove Duplicate Scripts](#remove-duplicate-scripts)
- [Configure ETags](#configure-etags)
- [Make Ajax Cacheable](#make-ajax-cacheable)
- [Flush the Buffer Early](#flush-the-buffer-early)
- [Use GET for AJAX Requests](#use-get-for-ajax-requests)
- [Post-load Components](#post-load-components)
- [Preload Components](#preload-components)
- [Reduce the Number of DOM Elements](#reduce-the-number-of-dom-elements)
- [Split Components Across Domains](#split-components-across-domains)
- [Minimize the Number of iframes](#minimize-the-number-of-iframes)
- [No 404s](#no-404s)
- [Reduce Cookie Size](#reduce-cookie-size)
- [Use Cookie-free Domains for Components](#use-cookie-free-domains-for-components)
- [Minimize DOM Access](#minimize-dom-access)
- [Develop Smart Event Handlers](#develop-smart-event-handlers)
- [Choose < link> over @import](#choose--link-over-@import)
- [Avoid Filters](#avoid-filters)
- [Optimize Images](#optimize-images)
- [Optimize CSS Sprites](#optimize-css-sprites)
- [Don't Scale Images in HTML](#dont-scale-images-in-html)
- [Make favicon.ico Small and Cacheable](#make-faviconico-small-and-cacheable)
- [Keep Components under 25K](#keep-components-under-25k)
- [Pack Components into a Multipart Document](#pack-components-into-a-multipart-document)
- [Avoid Empty Image src](#avoid-empty-image-src)

## Minimize HTTP Requests

减少 HTTP 的请求是提高你网页速度的关键。因为一个 HTTP 请求绝大多数的时间都消耗在建立连接和等待返回上。这里有一些减少 HTTP 请求的技巧

- **Combined files** - 把多个 JavaScript 文件或者多个 CSS 文件结合成一个文件，可以减少 HTTP 的请求
- **[CSS Sprites](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/css-sprites.md)** - 把你很多的背景图片结合成一张图片，然后用 `background-images` 和 `background-position` 来控制

## Use a Content Delivery Network

使用 CDN 可以大幅度的减少响应时间，CDN 是通过一系列分布在不同地域的 Web 服务器将服务器上的资源稳定高效的分发到用户手上。

> 因为当你发送请求的时候，会有一个将域名解析成 IP 的过程。如果你使用了 CDN，在 DNS 解析的时候，你就会经过一个智能调度 DNS 服务器，它可以从你请求的时候你所在的 IP 知道你所处在的位置，然后根据一定的算法，找到最接近你，最适合你的 CDN 节点 的 IP 返回给你

## Add an Expires or a Cache-Control Header

浏览器缓存的类型有 **强缓存** 和 **协商缓存** 两种

在浏览器去加载一个资源的时候，会先根据这一个资源上一次 HTTP 返回报文的 **Expires** 和 **Cache-Control** 这两个 header 来判断这个资源是否属于强缓存，如果属于，不向服务器发送请求，直接从缓存中加载，HTTP 状态码为 200，而返回的报文和数据都是上一次请求时保存起来的

如果不属于强缓存，浏览器就一定会发送这个请求，而服务器就会根据 HTTP 请求报文中的 **If-Modified-Since** 和 **ETag** 与服务器的这个资源的 **Last-Modified**  和 **If-None-Match** 判断是否属于协商缓存，如果属于，HTTP 状态码为 304，那么浏览器就又会在缓存中拿到我们的资源

如果不属于协商缓存，则正常请求，正常返回

## Gzip Components

GZip 压缩

## Put Stylesheets at the Top

CSS 应该放在 `head` 标签里

## Put Scripts at the Bottom

JavaScript 脚步应该放在 `</body>` 之前，但是并不是什么时候都可以，当该脚步需要用到 `document.write` 去把一些内容写到页面上的时候，我们就不可以把 JS 放到 `</body>` 之前

## Avoid CSS Expressions

在 CSS 里使用表达式 `expression()` 会动不动就要计算，页面渲染和页面大小改变的时候要计算，页面滚动的时候也要计算。不断地计算就会造成性能的损失

## Make JavaScript and CSS External

创建外部的脚步和样式文件不仅仅可以减少 HTML 文件的大小，而且脚步和样式文件还可以缓存起来，这样就可以减少请求数

## Reduce DNS Lookups

请求资源的时候，最好不要使用超过 2-4 个域，这样子 DNS 的查询就会变多，就会增加等待的时间

## Minify JavaScript and CSS

压缩我们的 JavaScript 和 CSS 可以减少我们的响应时间和下载时间

## Avoid Redirects

避免重定向

## Remove Duplicate Scripts

移除重复的脚本，不要加载两个相同的 JavaScript

## Configure ETags

[Add an Expires or a Cache-Control Header](#add-an-expires-or-a-cache-control-header)

## Make Ajax Cacheable

[Add an Expires or a Cache-Control Header](#add-an-expires-or-a-cache-control-header)

## Flush the Buffer Early

提前刷新缓存区

## Use GET for AJAX Requests

POST 和 GET 的区别

- POST 更安全
  	- 请求的参数不会作为 URL 的一部分
  	- 不会被缓存在浏览器中
  	- 不会保存在服务器日志中
  	- 不会存在浏览器历史记录中
- POST 发送的数据量更大
  	- GET 请求有 URL 的长度限制
- POST 能发送更多的数据类型
  	- GET 只能发送ASCII字符
- POST 比 GET 慢
  	- POST 请求会分开两步，先发请求头给服务器进行确认，然后才真正发送数据
  	- GET 会把数据缓存起来，而 POST 不会
  	- POST 请求包含更多的请求头

## Post-load Components

后加载组件

## Preload Components

预先加载组件

## Reduce the Number of DOM Elements

复杂的 DOM 结构意味着更多的 HTML 字节需要下载，也意味着 JavaScript 操作 DOM 的速度会更慢

## Split Components Across Domains

跨域分离组件

## Minimize the Number of iframes

减少 `iframes` 的数量

## No 404s

确保页面没有无效的请求，因为创建 HTTP 请求代价是昂贵的

## Reduce Cookie Size

HTTP cookies 中包含的信息会在服务器和浏览器之间的 HTTP 报文头部进行交换。太大的 cookies 会对响应时间造成影响

所以我们要保证 cookies 尽可能的小，设置合理的过期时间，会更快的移除不必要的 cookies 内容，缩短用户的响应时间

## Use Cookie-free Domains for Components

在请求静态资源的时候，没有必要把 cookies 也放过去，所以我们可以把我们所有的静态资源独立放在另一个设置了没有 cookies 的域里

## Minimize DOM Access

不仅仅查减少 DOM 的数量，还要减少对 DOM 的操作次数

## Develop Smart Event Handlers

使用 [Event Delegation](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/event-bubble-and-event-delegation.md) 或者操作 DOM 时，不必要等到 `window.onload` 方法，可以使用 `DOMContentLoaded` 事件，而它们的区别在 [window.onload vs $(document).ready](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/window-onload-vs-document-ready.md)

## Choose < link> over @import

使用 `<link>` 比 `@import` 要快

## Avoid Filters

不要使用 CSS 的过滤器

## Optimize Images

优化图片(Pixel-fitting)，压缩图片

## Optimize CSS Sprites

优化 CSS Sprites 

## Don't Scale Images in HTML

不要使用比你所需图片尺寸更大的图片，不仅仅是加载大小的问题，还是缩放时计算的问题

## Make favicon.ico Small and Cacheable

favicon.ico 应该保存在服务器根目录下，即使你不需要它，浏览器还是会去请求加载它。所以最好不要让请求返回 404。

## Keep Components under 25K

将组件维持在25k以下

## Pack Components into a Multipart Document

将组件放入复合的文档中

## Avoid Empty Image src

`src=''` 会使得浏览器向服务器发送另外的请求
