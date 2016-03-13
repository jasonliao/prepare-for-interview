# Something About < meta>

## Why We Need < meta> 

在过去，搜索引擎是靠 `<meat>` 标签里的属性值，如 `title`，`description`，`keywords` 等等来索引页面的，就是在页面上的排名。但是慢慢地，某些网站已经开始滥用它们，目的在于获得一个更好的搜索结果。所以现在，搜索引擎已经不会根据 `<meta>` 里面的属性来进行排名。

但是，它们仍然会出现在我们的搜索结果中，它就是我们的链接下面，方便用户去阅读和了解这网站的大概内容。我们可以优化我们的 `<meta>` 里的内容，来获取更大的点击率

## Usage

主要通过改变 `name` 属性来定义不同的 `<meta>`

- 描述 description，如果该标签内容为空，搜索引擎会从页面的内容中生成一段描述

    ```html
    <meta name="description" content="xxx" />
    ```
    
- 作用 author

    ```html
    <meta name="author" content="xxx" />
    ```
    
- 关键字 keywords，Google 不会在根据它来排名，但是百度却是一个重要因素

    ```html
    <meta name="keywords" content="xxx,xxx,xxx" />
    ```
    
还有一个 `http-eauiv` 属性，相当于 HTTP 头的作用

    
- 用于重写向和刷新 refresh，需要指定一个时间，但是 `url` 不是必须，如果是当前页面，则不需要填写 `url`

    ```html
    <meta http-equiv="refresh" content="5;url=xxx" />
    ```

- 用于告诉浏览器用何种版本渲染当前页面 X-UA-Compatible

    ```html
    <meta http-equiv="-UA-Compatible" content="IE=edga,chrome=1" />
    ```

- 缓存机制 cashe-control，还有另一个作用就是可以禁止百度对我们网页的转码
    
    ```html
    <meta http-equiv="cache-control" content="no-cache" />
    ```
    
    ```html
    <meta http-equiv="cache-control" content="no-siteapp" />
    ```

- cookie 的设定 set-cookie，可以设定我们的 cookie 和失效时间

    ```html
    <meta http-equiv="set-cookie" content="xxx=xxx;expires=xxx" />
    ```
    
最后，当然是我们最常用见的啦

```html
<meta charset="UTF-8" />
```