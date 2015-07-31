#  Difference Between Web Storage, Cookies and Session

> Web Storage(localStorage, sessionStorage) and cookies are all client storage solutions. 

> Session data is held on the server where it remains under your direct control.

## Web Storage

### sessionStorage

> sessionStorage (as the name persists) is only available for the duration of the browser session (and is deleted when the window is closed) - it does however survive page reloads

sessionStorage 在浏览器持续开启的时候，才会一直存在，一旦该浏览器窗口关闭，他里面的数据也会随之被删除，但是页面重新加载的过程不会把 sessionStorage 里的数据删除。

一个简单实用的例子，当页面意外重新加载的时候，我们可以从 sessionStorage 读取之前的东西。判断用户是否有勾取了自动保存

```javascript
var field = document.getElementById('field');

if (sessionStorage.getItem('autoSave')) {
	filed.value = sessionStorage.getItem('autoSave');
}

field.addEventListener('change', function () {
	sessionStorage.setItem('autoSave', field.value);
});
```

### localStorage

如果你存储的数据需要一直使用的话，那 localStorage 就会是更好的选择。

例如可以用来保存你来过这个网站多少次了

```javascript
var numberTime = localSession.getItem('numberTime');

if (numberTime == null) {
	numberTime = 0;
} else {
	numberTime = Number(numberTime);
}

numberTime++;
localSession.setItem('numberTime', numberTime.toString(10));
```

[localSession 的兼容性解决](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage#Compatibility)

### Between sessionStorage and localStorage

相同点

- 他们都可以用来存储信息
- 都可以被用户清除
- 存储在里面的数据都很容易通过客户端读出或者改变
- 都尽量不要存储敏感的，和安全相关的信息

不同点

- 存储的生命周期不同

Note: 不要过分依赖于在 localStorage 和 sessionStorage 里的数据

## Cookies

- Session Cookies - 这是一个暂时的 Cookies 文件，但你关闭浏览器之后，它就会被销毁，常用于购物车的应用
	
- Persistent Cookies - 这会一直在你的浏览器中直到你删除它，它可以记录你的登录信息和设置。常用登录验证和语言设置

Cookies 里的数据同样可以从纯文本中读取出来，不在万不得已下，不要把敏感的信息放在 Cookies 里面。如果你不使用 SSL，那么 Cookies 还会在传输的过程中被截取。

因为 Cookies 是用于引证用户信息的，所以每当页面发送请求的时候，包括原有的页面请求，Ajax 请求，所有的图片，样式，脚本等等，都会把 Cookies 一同发送到服务器端。

所以 Cookies 不应该用来存储大量的信息，不同的浏览器也会有不同的大小限制，一般来说， Cookies 用来存储引证信息，Session 和 广告追踪。

## Between Web Storage and Cookies

- Cookies 由服务端生成，一般情况下浏览器不会修改
- Cookies 只可以存储字符串
- sessionStorage 和 localStorage 就可以存储除了 Objects 和 Arrays 之外的 JavaScript 的函数，一般通过 API 去 存储 JSON
- sessionStorage 还可以让你存储符合你服务器端语言的任何函数对象
- Cookies 的容量限制在了 4KB
- 而 Web Storage 可以去到 5MB
- Cookies 一旦设置了之后，每个 http 请求都会带上 Cookies，而 Web Storage 不会
- 在存储和读取信息的时候，Web Storage 比 Cookies 简单得多

## Session

> As session data is completely controlled by your application (server side) it is the best place for anything sensitive or secure in nature.

## Reference

- [what is the difference between localStorage, sessionStorage, session and cookie?](http://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookie)
- [DOM Storage guide](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage#Compatibility)
- [Web Storage: Local Storage & Session Storage](http://www.9bitstudios.com/2013/10/web-storage-local-storage-session-storage/)
- [About Cookies](http://www.allaboutcookies.org/cookies/cookies-the-same.html)