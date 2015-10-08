# CSS Mastery - Part 2

Visual Formatting Model Overview

## Box Model

盒子模型详情可以看 [About * { box-sizing: border-box; }](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/about-box-sizing-border-box.md)

主要是标准盒子模型和IE盒子模型的区别还有设置 `box-sizing: border-box` 的一些好处

## Margin Collapsing

外边距的折叠主要发生在下面几个情况下

- 两个垂直的兄弟元素

    第一个的底外边距就会和第二个的顶外边距折叠，合成外边距较大的一个数值
    
- 父元素与他的第一个子元素

    当父元素和第一个子元素的 `padding` 和 `border` 都没有的情况下，父元素的外边距就会是他和子元素外边距之中的较大值

- 没有 `padding` 和 `border` 的空元素
    
    这时候就会自身发生折叠

## Positioning

### Normal Flow

行内元素不可以通过 `width` 和 `height` 来设置大小，对 `padding-top` `padding-bottom` `margin-top` `margin-bottom` `border-top` `border-bottom` 也不起作用，唯一可以行内元素尺寸的方法就是修改行高 `line-height` 和水平方向上的边框，内边距和外边距 `padding-left` `padding-right` `margin-left` `margin-right` `border-left` `border-right`

### Relative Positioning

设置了相对布局之后 `position: relative` 元素还是会在他原来的位置，这时候可以设置他的 `top` `right` `bottom` `left` 属性使他相对于原来位置进行偏移，但是不管偏不偏移，元素仍会占有空间，因此也可能会导致元素可能会覆盖其他元素

### Absolute Positioning

设置了绝对定位之后 `position: absolute` 元素就会脱离文档流，不会占有空间

绝对定位的元素的位置是相对于距离它最近的那个已经定位的祖先元素开始，如果元素没有已定位的祖先元素，那么它的位置就是相对于初始包含块的

### Fixed Positioning

固定定位 `position: fixed` 可以把元素设定在视口的固定位置浮动

## Floating

浮动会使元素脱离文档流，后面的元素不会再受该元素的影响，但是后面的元素中的文本却会受浮动元素的影响，围绕在浮动元素的周围

设置了浮动之后，可能会涉及到其他的问题，这时我们可以考虑清理浮动，而清理浮动的几个方法可以看 [Clearing Floats](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/clearing-floats.md)