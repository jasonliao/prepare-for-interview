# How JSONP Works

JSONP 是用来解决跨域请求的，它允许用户传递一个 `callback` 给服务器端，然后服务端返回数据时会将这个 `callback` 作为函数名来包裹住 JSON 数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了

凡是拥有 `src` 属性的标签都可以跨域，JSONP 靠的也是这个，我们用 `<script>` 来实现 JSONP

## JSONP in Native

> 然后服务端返回数据时会将这个 `callback` 作为函数名来包裹住 JSON 数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了

这是什么意思？我们先来把代码写死看看

```html
<!-- 
  先有一个 local.html
  跑在 localhost:4000 上
-->
<body>
  <script>
    // 定义了一个处理函数
    function localFunction (data) {
      console.log(data);
    }
  </script>
  <script src="http://localhost:3000/remote.js"></script>
</body>
```

```javascript
// 然后有一个 remote.js
// 跑在 localhost:3000 上
var jsonData = {
  name: 'Jason',
  age: 20
};
// 调用之前定义的处理函数
localFunction(jsonData);
```


这时我们可以很清楚我们在做什么了，我们请求了一个 `remote.js`，它就调用了我们在 `local.html` 里定义的 `localFunction()`，并把 `jsonData` 数据作为参数。

因为 `localFunction()` 是我们在本地自己写的，而 `jsonData` 是其他域的数据，我们可以把其他域的数据用本地写好的处理函数调用，这就相当于我们从其他域中拿到了数据了，Yep!

但是问题来了，`remote.js` 里怎么知道我们处理 `jsonData` 的函数的名字就是 `localFunction()` 呢？

它不知道，那我们就告诉它吧，我们可以在请求他的时候把要处理数据的函数的名字告诉它

```html
<!-- local.html -->
<script src="http://localhost:3000/remote.js?callback=localFunction"></script>
```

这时候我们还要把 `remote.js` 大改，把它改成服务器端的文件

```javascript
// remote.js
var http = require('http');

var jsonData = {
  name: 'Jason',
  age: 20
};

var server = http.createServer(function (req, res) {
  var url = req.url,
      qs = url.split('?')[1],
      callback = qs.split('=')[1];

  res.end(callback + '(' + JSON.stringify(jsonData) + ')');
});

server.listen(3000);
```

不管你是用 node 也好，什么语言都好，只要把字符串拼好返回就可以了，这时你就可以在浏览器控制台里看到我们打印的 `jsonData`

## JSONP in jQuery

虽然 JSONP 和 Ajax 不是同一个东西，**Ajax 核心的依赖是浏览器提供的 `XMLHttpRequest` 对象**。但是 jQuery 还是把他们绑在了一起，那我们用 `$.ajax()` 怎么写呢

```javascript
$.ajax({
  url: 'http://localhost:3000/remote.js?callback=?',
  dataType: 'jsonp',
  success: function (data) {
    console.log(data);
  }
});
```

当我们使用 `dataType` 为 `'jsonp'` 的时候，就要在我们的 `url` 后面添加 `?callbacl=?`，当你真正发送请求的时候，`?` 就会变成 jQuery 自动生成的随机函数名，这个函数名的指向，就是我们写在 `success` 里的函数，当请求成功时，其他域的数据就会传入 `data` 参数，这样就会执行我们写在 `success` 函数里操作了

所以这时候，我们在 `remote.js` 里只需要返回数据就可以了

```javascript
res.end(JSON.stringify(jsonData));
```

当然这时候你还是去请求那个不健硕的 `remote.js` 的时候，浏览器就会报错了

```bash
Uncaught ReferenceError: _ is not defined
```

然后你看请求的时候就会发现，原来不仅仅发送了一个参数，还有另一个参数 `_=一串数字`，所以 `remote.js` 解析的时候，没有把第二个参数考虑到

你可以在 `remote.js` 中添加更加健硕的代码，或者直接在 `$.ajax()` 里把 `cache` 为 `true`，因为那个 `_` 传过去的参数是一串随机数，用来确保没有缓存的