# window.onload vs $(document).ready

## window.onload

`window.onload` 就是当所有的资源加载完后再触发，包括 DOM，异步的 JavaScript，frames 和图片，是 JavaScript 原生的方法，有两种使用的方法

1. 直接在 JavaScript 中调用 `window.onload`

    ```javscript
    window.onload = function () {
      // ...
    };
    ```
    
2. 在 HTML `body` 里添加 `onload` 事件

    ```html
    <body onload="onloadFunction()">
    ```
    
    ```javascript
    function onloadFunction () {
      // ...
    }
    ```

## $(document).ready

`$(document).ready()` 是 jQuery 的方法，在 DOM 加载完后就立刻触发，在 `window.onload` 之前。

```javascript
$(document).ready(function () {
  // ...
});
```

## $(window).load

`$(window).load` 和 `window.onload` 的执行的时间是一样的，但它也是属于 jQuery 的方法

```javascript
$(window).load(function () {
  // ...
}); 
```

## DOMContentLoaded

`DOMContentLoaded` 和 jQuery 的 `$(document).ready` 的执行时间一样，也是当 DOM 加载完后就立刻执行

```javascript
document.addEventListener('DOMContentLoaded', function (event) {
  // ...
});
```

HTML5 标准，IE9+

## readyState

`readyState` 有三个值，分别是 `loading`，`interactive` 和 `complete`。 `interactive` 对应的是 `document` 的 `DOMContentLoaded` 事件，而 `complete` 对应的就是 `load` 事件

所以要想达到 `$(document).ready` 的效果，也可以这样使用

```javascript
document.onreadystatechange = function () {
  if (document.readState === 'interactive') {
    // ...
  }
}
```

而如果想达到 `$(window).load` 的效果，则可以这样

```javascript
document.onreadystatechange = function () {
  if (document.readState === 'complete') {
    // ...
  }
}
```