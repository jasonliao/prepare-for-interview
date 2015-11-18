# Interview Questions

> 什么是 css reset

为了让一些没有样式的网页看起来更加清晰，每个浏览器引擎都会有他们的默认的样式，也正因为这样，我们的网页会在不同的浏览器引擎下有不同的表现

用了 css reset 之后，就可以尽可能地把这些差异减少到最低

[css reset](http://cssreset.com/) 这个网站给我们提供最受欢迎的 css reset

> 你平常写 css 的时候有考虑过 css 的性能么

- 尽量避免使用 `*` 通配符
- 减少使用 css 表达式
- 不要多个选择器同时使用

> 你知道什么是CSS预处理么

css preprocessor 其实是一种脚本语言，它可以把我们按照一定语法写的 "css" 转成常规的 css

1. 为什么要使用 css preprocessor？

    - 代码更清晰，重用性更高
    - 可以更加灵活地写 css，有利于减短开发时间
    - 可以共用别人写好的代码或库，只需要简单的 `import`
    - 更容易写出跨浏览器的 css

2. 有哪些流行的 css preprocessor？

    - sass
    - less
    - stylus
    
> 关于浮动的原理和工作方式，你可以描述一下么

浮动和绝对定位一样，都会脱离文档流，然后再按照浮动的值，也就是 `left` 或者 `right` 来确定元素向哪个方向浮动

Note: 绝对定位会比浮动更高

> 浮动会产生什么影响呢，要怎么处理

浮动之后一般会产生父元素的折叠，所以我们要 [清理浮动](https://github.com/L-movingon/prepare-for-interview/blob/master/CSS/clearing-floats.md)

> 你了解哪些选择器？以及这些选择器的使用场景

[Selectors](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-1.md#selectors)

一般的选择器会用来选取元素，为元素添加 css 样式

像 `:hover` `:active` `:focus` 这些动态伪类可以用来为元素添加一些动态效果

而 `:before` `:after` 这些伪元素，就可以为原来的元素添加一些小东西，例如标签的小三角呀，标题的多条下划线呀

而属性选择器做得也更多啦，属性是我们自己定义的，所以我们可以进一步细化我们的样式，例如不同属性的 `a` 标签可以有不同的样式告诉用户这个链接是什么类型的等等

> 你知道它们的权重怎么计算么

[Specificity](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-1.md#specificity)

> 你了解哪些布局？你平时有使用过什么布局实现

- [固定布局](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-4.md)
- [流式布局](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-4.mdhttps://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-4.md)
- [弹性布局](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-4.mdhttps://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-4.mdhttps://github.com/L-movingon/prepare-for-interview/blob/master/Books/CSS-Mastery/css-mastery-part-4.md)
- flex布局
- [grid布局](https://l-movingon.github.io/posts/2015-10-11-grid-layout.html)

> 对于js你平常用什么框架

- jQuery
- ExtJS
- React

> js有哪些数据类型呢

1. 简单数据类型

    - 数值
    - 字符串
    - 布尔值
    - undefined
    - null
    
2. 复杂数据类型

    - 数组
    - 对象

> 这些数据类型，哪些是引用类型的呢

- 数组
- 对象

> 你知道原型链么

`__proto__` 就是原型链

引出 [继承](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md)

> 说说函数表达式和函数声明的区别

[Function Declaration and Function Expression](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/difference-between-function-declaration-and-function-expression.md)

> 你知道闭包么，为什么要使用闭包？

- 闭包可以读到外部函数的变量，所以可以用来创建私有变量
- 闭包可以让外部变量一直存在内存中，可以用来缓存我们的数据

> 你知道 attribute 和 property 的区别么

- property 是标签默认的属性才有
- attribute 是全部默认的加上自己定义的

> 你有了解过作用域链么

作用域链其实就是所有内部上下文的变量对象的列表

[Scope Chain](https://github.com/goddyZhao/Translation/blob/master/JavaScript/%E4%BD%9C%E7%94%A8%E5%9F%9F%E9%93%BE%EF%BC%88Scope%20Chain%EF%BC%89.md)