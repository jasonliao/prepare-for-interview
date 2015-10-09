# CSS Mastery - Part 4

Layout & Bugs and Bug Fixing

## Fixed-width, Liquid, and Elastic Layout 

固定宽度布局可以使开发人员对页面有更大的控制能力，但是有分辨率越来越高，和移动端越来越普及的今天，已经越来越不适合

所以我们可以用流动布局和弹性布局来代替

## Liquid Layout

流式布局是把尺寸由像素换成百分比的布局，可以根据浏览的大小变化而变化，更高效的利用浏览器，但是当浏览器的窗口变得很小很小的时候，列也会变得很小，这样也很难阅读，所以还可以添加一个以像素为单位的 `min-width`，但是当 `min-width` 设置得很大的时候，就是会遇到和固定宽度布局一样的问题了

创建流式布局的时候，可以把设计的宽度与用户最常用的浏览器窗口大小 1250 相除，求出百分比。而里面的内容也用相同的方法，把所有的固定像素转成百分比即可

## Elastic Layout

弹性布局解决的是当浏览器缩放的时候，也就是当字体缩放的时候，布局也会有良好的表现。

把固定宽度布局转到弹性布局还是很简单的，技巧是设置基字号，让 1em 大致相当于 10 像素

```css
body {
    font-size: 62.5%;
}
```

然后把最外层的容器设置成 em 为单位的宽度，而容器里的内容仍然使用百分比就可以了，如上面的 `wrapper`，当然我们还可以设置 `max-width` 为 100% ，防止窗器比浏览器的窗口大而产生水平滚动条

```css
.wrapper {
    width: 92em;
    max-width: 100%;
}
```

## Conditional Comments

```html
<!--[if IE 6]>
    .... some HTML here ....
<![endif]-->
```

| 符号 | 例子 | 解释 |
| :----: | :----: | :----: |
| ! | [if !IE] | 非 |
| lt | [if lt IE 5.5] | 小于 |
| lte |	[if lte IE 6] | 小于等于 |
| gt | [if gt IE 6] | 大于 |
| gte | [if gte IE 5.5] | 大于等于 |
| & | [if (gt IE 5)&(lt IE 7)] | 并 |
| ｜ | [if (IE 6)｜(IE 7)] | 或 |

## Read More

- [About conditional comments](https://msdn.microsoft.com/en-us/library/ms537512\(v=vs.85\).aspx)