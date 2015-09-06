# Clearing Floats

## Why

之所以要清除浮动是因为它们会影响父元素的的表现，如果这个父元素里面装的全都是浮动的元素，那么这个父元素的高度就会变成 0

## How

1. The Empty Div Method

	就是简单的用一个空的 `<div style＝"clear: both"></div>`，使用 `div` 的原因是它没有任何的浏览默认样式，所以不用担心不同的浏览器会有不同的表现
	
	但是在要在浮动元素之后，父元素结束之前加上。
	
	但是却违背了 HTML 的语义化。
	
2. The Overflow Method
	
	可以对父元素设置 `overflow: auto/hidden`，这样就可以使得父元素的高度被浮动的元素扩展。
	
	这是因为加上了这个属性之后，会使得父元素变成 [BFC](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/block-formatting-context.md)，因为 [BFC](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/block-formatting-context.md) 是可以包含浮动的，所以就会有了高度。
	
	但也要注意的是，`overflow` 属性可能会让你的内容看不见或者出现了你不想要的滚动条

3. The Pseudo Selector Method

	可以使用 CSS 的伪类 `:after` 来清除浮动。在父元素加入 `clearfix` 这个类。
	
	```css
	.clearfix:after {
		content: '.';
		visibility: hidden;
		display: block;
		height: 0;
		clear: both;
	}
	
	.clearfix {
		zoom: 1;	/* IE 6/7 */
	}
	```
	
4. The Future `contain-float` Value
	
	W3C 准备加入这个 `contain-float` 到 `min-height` 和 `max-height` 这个 CSS 属性上。详情看[这里](https://drafts.csswg.org/css-sizing/#the-contain-floats-value)
	
	主要就是为了解决这个问题，那我们只要在父元素上这个这个属性值即可
	
	```css
	.container {
		min-height: contain-float;
	}
	```
	
## Reference

- [All About Floats](https://css-tricks.com/all-about-floats/)
- [Clearing Floats: An Overview of Different clearfix Methods](http://www.sitepoint.com/clearing-floats-overview-different-clearfix-methods/)
