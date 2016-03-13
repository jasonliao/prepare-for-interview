# Why We Need Sass

Sass 给我们提供了很多特性，可以减少我们的代码量和开发时间，而且 CSS 里的所有语法，在 Sass 里都是合法的，所以尽管你只使用 Sass 里的一点点特性，都可以使用 Sass

## Nesting

我们可以选择器嵌套，也可以属性嵌套

```css
$linkColor: #08C #333; // 一个默认值，第二个是悬浮的值

a {
  color: nth($linkColor, 1); // nth 是 Sass 提供的函数
  &:hover {
    color :nth($linkColor, 2);
  }
}
```

## Variables

我们常常在写 CSS 的时候，会经常翻看之前的样式，来找我们用过的顔色，用过的字体，但在 Sass 里面你可以为这些常用的东西定义成变量，我们可以随时调用

```css
$mainFont: "Helvetica Neue";
$maniColor: #CC6699;

.mySelector {
  font-family: $mainFont;
  color: $mainColor;
}
```

当然我们的变量不仅限于一般的属性值，还可以是其他的值，例如 `top`，`left` 这些，可以用来当我们的属性，但是需要 `#{$variables}` 来使用

```css
$borderDirection: top;

.border-#{$borderDirection} {
  border-#{$borderDirection}: 1px solid #ccc;
}
```

还有一点可以使用变量来计算获取值，同样是使用 `#{$variables}`

```css
$baseFontSize: 12px;
$baseLineHeight: 1.5;

body {
  font: #{$baseFontSize} / #{$baseLineHeight};
}
```

当然，现在原生 CSS 草案也支持变量，但是对于现在来说，很多东西还没有完善，浏览器也没有完全兼容，可以 Sass 的变量给我们提供了很大的便利

## @import

CSS 的 `@import` 是把另一个样式引入进来，它会发起另一个 HTTP 请求，我们是都应该避免在 CSS 里使用 `@import`

而 Sass 则会在编译的时候，先去编译我们引入的文件，这可以驱使我们把样式分开来写，更加合理化，模块化且条理化，用法和 CSS 的使用方法一样。但是我们的文件命名就特殊一点，我们的局部文件需要在文件名前加 `_`，那么 Sass 就不会把我们这个文件编译成 CSS 

## Color Functions

Sass 给我们提供了很多对于顔色的函数，我们可以直接调用，如 `lighten($color, $amout)`，`darken($color, $amount)` 和 `rgba($color, $alpha)`

## Mixins

我们可以利用 `mixin` 来定义一组 CSS 的表达式，一般用来解决我们的兼容问题，也解决了不断重复输写，增加代码量的问题

```css
@mixin box-sizing($boxSize) {
  -webkit-box-sizing: $boxSize;
     -moz-box-sizing: $boxSize;
          box-sizing: $boxSize;
}

.mySelector {
  @include box-sizing(border-box);
}
```

## @extend

这个特性可以让我们把之前写过的选择器的样式应用到其他选择器中。例如我们有两个按钮，两个按钮的样式基本相同，但是另一个却要求背景色不一样

```css
.button {
  background: rgba($color, 0.8);
  color: lighten($color, 65%);
  border: 1px solid darken($color, 5%);
  padding: 1em;
  display: block;
  max-width: 200px;
}

.button-decline {
  @extend .button;
  background: red;
}
```

又是一个可以节省我们开发时间和减少我们代码量的好特性。

一句话说，之所以使用 Sass，就是为了 **DON'T REPEAT YOUSELF** 