# CSS Mastery - Part 1

## Navigation

```html
<nav>
    <li><a href="/home/">Home</a></li>
    <li><a href="/about/">About</a></li>
    <li><a href="/contact/">Contact</a></li>
</nav>
```

把导航栏中不同的部分单独放在不同的文件夹里，每个文件夹里各有一个 `index.html` 而不是直接有 `about.html` `contact.html` 这样会对路由更加友好，也有助于程序的扩展

## Naming Your IDs and Classes

应该根据 "What the are" 来命名，而不是 "How they look"

一般采用完全小写，多个单词之间就用连字符分隔 如 `main-nav` `page-nav` `footer-nav`

## IDs or Classes

**只有绝对确定这个元素只会出现一次的情况，才应该使用 id**

## Do Not Overused Class

如果你发现类名中出现了重复的单词，比如 `news-head` `news-link` `news-text` 那就应该考虑是不是可以把他们组成一个部分，然后让 CSS 选择器来控制我们下面的子元素

减少不必要的类可以简化我们的代码，使我们的页面更加的简洁

## div and span

`div` 其实是有语义的，是 division 部分的意思，所以我们可以把整个文档分割几个有意义的部分，或者叫区域。但是却不可以滥用，应该只有在没有现有元素能够实现区域分割的时候，才使用 `<div>`，例如导航栏的列表，就不需要包在 `<div>` 中


```html
<div class="nav">
    <ul>
        <li><a href="/home/">Home</a></li>
        <li><a href="/about/">About</a></li>
        <li><a href="/contact/">Contact</a></li>
    <ul>
</div>
```

```html
<ul class="nav">
    <li><a href="/home/">Home</a></li>
    <li><a href="/about/">About</a></li>
    <li><a href="/contact/">Contact</a></li>
<ul>
```

## Selectors

- 元素选择器
- 后代选择器
- ID选择器
- 类选择器
- 伪类选择器

    `:link` 和 `:visited` 称为链接伪类，只能用于 `<a>` 标签，`:hover` `:active` `:focus` 称为动态伪类，理论上可以应用于任何元素，但是 IE 却对他们的支持各不相同，在　IE7 后就支持 `:hover` 
    
- 通用选择器
- 高级选择器

    高级选择器对 IE6 或更后的版本不支持，但是对高级选择器不支持的浏览器会忽略整个规则，只要不要在站点功能或布局很重要的元素上使用高级选择器就可以了
    
    - 子选择器
    - 同胞选择器
    - 属性选择器

### Attribute Selectors

属性选择器可以根据某个属性是否存在或属性的值的寻找元素，例如我们可以用来实现鼠标悬停时，提示框的弹出或最简单的更换 `cursor` 的样式
    
```css
// 若 <a> 中有 title 属性
a[title] {
    cursor: help;
}
```

还可以根据浏览器对属性选择器的支持来对浏览器进行样式设置

```css
#header {
    background: black;
}
[id="header"] {
    background: #fafafa;
}
```
 
这样就可以把不支持属性选择器的低版本的浏览器(IE6)的背景色设置成黑色

属性选择器还可以与类似正则的语法一起使用

- a[title*="xxx"] 选择所有 `title` 属性中值包含有 `xxx` 的 `<a>` 标签
- a[title^="xxx"] 选择所有 `title` 属性中值以 `xxx` 开头的 `<a>` 标签
- a[title$="xxx"] 选择所有 `title` 属性中值以 `xxx` 结尾的 `<a>` 标签
- a[title~="xxx"] 选择所有 `title` 属性的多个值中含有 `xxx` 这个值的 `<a>` 标签

## Specificity

一般我们会有一个规则来计算选择器的特殊性

选择器的特殊性若以10为基数分成 4 个等级

- 行内元素样式 1000
- ID选择器 n x 100
- 类选择器，属性选择器，伪类选择器 n x 10
- 元素选择器，伪元素选择器 n x 1

| 选择器 | 特殊性 | 以10为基数的特殊性|
| :----  | :----: | :----: |
|style=""| 1,0,0,0  | 1000 |
| \#wrapper \#content {} | 0,2,0,0 | 200 |
| body #content div[id="main-content"] h2 {} | 0,1,1,3 | 113 |
| \#content .datePosted {} | 0,1,1,0 | 110 |
| div#content {} | 0,1,0,1 | 101 |
| p.content .datePosted {} | 0,0,2,1 | 21 |
| p {} | 0,0,0,1 | 1 |

## Applying Styles to Your Document

- 可以在 html 里的 `<style>` 下导入样式

```html
<style>
    @import url(/css/layout.css);
</style>
```

- 可以直接用 `<link>`　链接样式

```html
<link rel="stylesheet" href="/css/layout.css">
```

但是链接样式比导入样式要快

## Structuring Your Code

要把 css 文件结构化，这样在维护的时候，就会简单得多

- 一般性样式
    - 主体样式
    - reset 样式
    - 链接
    - 标题
    - 其他元素
- 辅助样式
    - 表单
- 页面结构
    - 页头，页脚和导航
    - 布局
    - 其他页面结构元素
- 页面组件
    - 各个页面
- 覆盖

可以在 css 文件中添加注释来区别每个部分

```css
/* @group general styles
------------------------------- */

/* @group helper styles
------------------------------- */

/* @group page structure
------------------------------- */

/* @group page components
------------------------------- */

/* @group overrides
------------------------------- */
```

## Make A Good Comments

```css
/* @todo: 以后需要做的东西 */
/* @workaround: 暂且这样做，但是并不完善 */
/* @bugfix: 表示代码或者浏览器遇到的问题 */
```

根据 cssdoc，一般会在 css 文件之前加上项目名，版本号，作者和联系方式

```css
/**
* Homepage Style
*
* Description
*
* @project:
* @version:
* @author:
* @contact:
*/
```

##Read More
- [Manageable CSS With CSSDOC](http://timkadlec.com/2008/12/manageable-css-with-cssdoc/)