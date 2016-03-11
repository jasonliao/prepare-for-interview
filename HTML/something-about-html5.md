# Something About HTML5

## 语义

随着 HTML 和 CSS 的逐渐分离，我们编写 HTML 的时候越来越注重语义化，在 HTML5 中，我们有更丰富更有含义的标签

- 在内容方面
    - `<article>` 表示一篇文章
    - `<audio>` 表示音频
    - `<video>` 表示视频
    - `<canvas>` 表示画布
    - `<time>` 表示日期／时间
- 在结构方面
    - `<aside>` 表示侧边栏
    - `<nav>` 表示导航
    - `<header>` 表示头部
    - `<footer>` 表示底部
    - `<section>` 表示一个区域
- 表单方面
    - `type="emil"`
    - `type="date"`
    - `type="color"`
    - `required`，`max` 表单的验证

不仅仅增加了标签，还减少了很多关于样式的标签

- `<big>`
- `<center>`
- `<font>`
- `<u>`

HTML5 的新标记不仅仅可以使网页更加的语义化，而且对搜索引擎也会更加的友好

## 离线与存储

使用 Web Storage，包括 [sessionStorage 和 localStorage](https://github.com/L-movingon/prepare-for-interview/blob/master/HTML/difference-between-webstorage-cookies-and-session.md#web-storage) 进行存储数据，相比于 cookie 有更好的操作 API，有更大的容量，而且还有更优化的性能

而 Service Workers 则可以做到离线时仍能从缓存从获取我们的脚步和样式


## 跨文档消息传送

HTML5 有 `window.postMessage` 这个方法可以给我们安全的实施跨文档的消息传递。它可以给指定的域发送信息，当目标域接收到信息的时候，就会触发 `window` 的 `message` 事件，传进来的事件对象有三个属性

- `data` 就是发送过来的信息
- `origin` 就是发送方所在的域
- `source` 就是发送方的 `window` 对象代理

## 拖放

HTML5 给我们原生的拖放 API，对于被拖动元素，会有以下三个事件发生

- `dragstart` 按下鼠并开始移动鼠标时
- `drag` 拖动时持续触发
- `dragend` 拖动停止时

而对于放置目标

- `dragenter` 拖动元素进入放置目标
- `dragover` 在放置目标内移动
- `dragleave` 拖出了放置目标范围
- `drop` 放置在目标范围内

只要重写一个元素的 `dragenter` 和 `dragover` 事件，就可以使它变成一个有效的放置目标

## 多媒体

`<video>` 和 `<audio>` 可以嵌入视频和音频，它们不仅仅有属性可以给我们知道媒体当前的状态，还有很多的事件可以给我们操作这个播放器

## 3D 与图像动画

HTML5 里最受欢迎的功能就是 `<canvas>` 画布，我们可以使用 JavaScript 动态在在这个区域绘制图形动画，那是因为 `<canvas>` 给我们提供的 2D 对象，我们可以通过 `canvas.getContext('2d')` 来获取

很多手机网页的游戏，都是通过 `<canvas>` 和 SVG 来创建，慢慢取代了 flash 做的游戏

## 地理位置 API

HTML5 给我们提供了 `Geolocation` API，`geolocation` 对象放在了 `navigator` 对象里。

最常用的方法就是 `getCurrentPosition`，这个方法有两个参数，第一个是成功的处理函数，第二个则是出现错误的处理函数。

```javascript
navigator.geolocation.getCurrentPosition(success, error);
```

如果成功，则会传入一个 `position` 参数

- `coords`
    - `latitude` 纬度
    - `longitude` 经度
    - `altitude` 海拔
    - `heading` 度(顺时针，以正北为基准)
    - `speed` 米/秒
- `timestamp`

```javascript
function success (position) {
  var latitude = position.coords.latitude,
      longitude = position.coords.longitude;
  console.log('Your position: Lon: %s, Lat: %s', longitude, latitude);
}
```

## 设备访问

- `orientationChange` 事件 

    判断设备是否切换为横向查看模式
    
    ```javascript
    window.addEventListener('orientationChange', updateOrientation, false);
    
    function updateOrientation () {
      switch (window.orientation) {
        case: 0: // 肖像模式
        case: 90: // 向左旋转
        case: -90: // 向右旋转
      }
    }
    ```

- `deviceorientation` 事件

    三维的旋转角度，`beta` 是 x 轴方向，`gamma` 是 y 轴方向，`alpha` 是 z 轴方向
    
- `MozOrientation` 事件 
    
    Mozilla 对 `deviceorientation` 的兼容支持

- `devicemotion` 事件

    除了方向信息以外，还有加速度的信息
    
除了这些设备的操作事件之外，还有触控和手势的事件，详情可以看这里 [Touch Events](https://developer.mozilla.org/en-US/docs/Web/API/Touch_events)