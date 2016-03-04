# Reflow and Repaint

## Reflow

### What is reflow?

当你改变一个元素的几何大小和定位的时候，页面的整个 "元素流" 也会跟着变化，这就是我们说的 reflow

reflow 在性能上的代价是大的，是因为当你改变一个元素的大小和定位的时候，这个元素的子元素、祖先元素和所有底下的元素都要去重新计算大小和定位。在大多数情况下，reflow 实际上是会导致页面的重新渲染

而在手机上更是明显，相比于 PC 低效的处理器和较小的屏幕，页面常常会整个页面重新布局

### What causes a reflow?

很不幸的是，很多东西都会导致 reflow，下面是常见的一部分

- 改变布局
- 浏览器窗口的变化
- 增删改 DOM 元素
- 增删改关于大小和定位的 CSS 样式
- 改变字体或字体大小
- CSS3 动画
- 计算偏移高度或偏移宽度
- 使用伪类如 `:hover`

### How to avoid reflows

如果不能避免 reflow，我们应该尽可能地减少次数和时间，下面有几个方法

- 在绝对和固定定位的元素上使用动画可以减少 reflow 的时间，因为绝对和固定定位的元素都已经脱离了文档流，所以对文档流里的元素影响较少
- 添加 DOM 节点的时候，不要一个一个的插入，这只会导致一次又一次的 reflow，所以应用使用 `fragment` 来包裹节点，再进行一次插入操作
- 修改元素样式的时候，不应该一个一个的设置，而更应该为样式设置类，我们只要对类进行操作就可以了，且把类应用在最靠近该元素的元素上
- 降低 DOM 的嵌套层数，正如上面所说任何一个元素的几何改变，都会导致子元素和祖先元素的重新计算，所以减少DOM 的嵌套层数有助于降低 reflow 的时间
- 减少不必要的 CSS 规则和不要使用过于复杂的 CSS 选择器
- 使用动画时，可以把每次移动 1px 改成 3px

## Repaint

### What is repaint?

当你改变一个元素的样子却不改变他的大小和形状的时候，这不会导致 reflow 因为元素的几何形状没有发生改变

### What causes a repaint?

修改元素的 CSS 样式例如 `background-color`，`opacity`，`visibility` 和 `outline` 等等

repaint 对于浏览器性能的代价也很大，因为浏览器每次 repaint 都必须检查 DOM 中所有元素的可见性，但相比于 reflow，repaint 代价较低

## Reference

- [Repaint and Reflow](https://dev.opera.com/articles/efficient-javascript/?page=3#reflow)
- [Reflows & Repaints: CSS performance making your javascript slow?](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/)
- [Minimizing browser reflow](https://developers.google.com/speed/articles/reflow)
- [10 Ways to Minimize Reflows and Improve Performance](http://www.sitepoint.com/10-ways-minimize-reflows-improve-performance/)
- [CSS Triggers](https://csstriggers.com/)