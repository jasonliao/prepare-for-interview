# CSS Mastery - Part 3

Using Backgrounds for Effect & Styling Links

## Background Image

`background-position` 的值可以是 `left/right/center top/bottom/center`，也可以是 `x% y%`，还可以是具体的 `xpx ypx`

当值为具体的多少像素的时候，图像的左上角就是会出现在离元素左上角的具体像素数，若值为百分数的时候，就会是距离图像具体百分比的点到元素左上角具体百分比的距离

## Opacity

我们可能要实现一个这个的需求，有一个警告框弹出警告消息，他会覆盖在我们的文档上面，但同时，也可以看到下面文档的内容，我们可能会这样做

```css
.alert {
    background: #000;
    border-radius: 2em;
    opacity: 0.8;
    filter: alpha(opacity=80);
}
```

但是这样会导致一个问题，警告框里的文本也会变成 80% 的不透明，所以我们就需要 `rgba`

`rgba` 分别代表 red green blue 和 alpha，所以我们就可以通过下面代码来把我们的警告框的背景设置成透明，但却不会对其文本有影响

```css
.alert {
    background: rgba(0, 0, 0, 0.8);
    border-radius: 2em;
}
```

## PNG Transparency

IE6 为了可以显示 PNG 的透明通道，他们有专有的 CSS 扩展 － behavior。下载合适的 `.htc` 文件并在 IE6 专用的样式表中引用它，就可以在任何元素上雇用 PNG 透明度

```css
img, div {
    behavior: url(iepngfix.htc);
}
```

## Simple Link Styling

我们可以简单的使用 `a { color: red }` 使得所有的 `<a>` 标签都变成红色，但是 `<a>` 标签不仅仅可以用作外部链接，还可以用作内部的引用，例如

```html
<p><a href="#main-content">Skip to the main content</a></p>
<h1><a name="main-content">Welcome</a></h1>
```

如果我们简单的对 `a` 进行样式应用，那么标题也会变成红色。为了解决这个问题，CSS 给我们提供了 链接伪类 `:link` `:visited`

这样我们就可以通过设置 `a:link { color: red }` 来确保链接变成红色

同时 CSS 还提供了动态伪类 `:hover` `:focus` `:active` 

`:focus` 是当键盘移动到这个链接上的时候的样式

对 `<a>` 添加样式的时候，应该按照下面的样式顺序

`:link` `:visited` `:hover` `:focus` `:active`

> Lord Vader Hates Furry Animals

## Styling Link Targets

`<a>` 标签中的 `href` 还可以链接到某个页面的某一个部分，实现的方法就是在链接的后面加一个 `#`，再加上要链接元素的ID

所以我们可以为跳到的元素元素设置样式，使用 `:target` 伪类

例如我们可以为这个元素设置动画背景图像，图像从黄色褪色到无色

```css
.comment:target {
    background: url(img/fade.gif);
}
```

## Highlighting Different Types of Links

我们可以通过属性选择器去区别 `<a>` 标签中 `href` 属性的值，实现此功能就是靠属性选择器与类似正则表达式的一些运用，[Part 1](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-1.md) 中有详细的说明， 下面可以一些例子

```css
a[herf^="http:"] {
    // 这是外链的样式
}

a[href^="mailto:"] {
    // 这是写邮件的样式
}

a[href$=".pdf"] {
    // 这是看pdf的样式
}
```

## Pure CSS Tooltips

```html
<p><a href="https://l-movingon-github.io/" class="tooltip">Jason's<span>This BLog is hot</span></a> is my blog</p>
```

```css
a.tooltip {
    position: relative;
}
a.tooltip span {
    display: none;
}

a.tooltip:hover span {
    position: absolute;
    display: block;
    // tooltip pop up style
}
```