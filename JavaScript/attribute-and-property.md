# Attribute and Property

> Attributes are defined by HTML. Properties are defined by DOM.

## What is a property

不同的标签会有不同的 property，这些 property 可以有不同的类型，例如布尔值，字符串等等。property 可以通过 jQuery 的 `prop` 方法来获取

```javascript
<a href="index.html" class="link" name="linkName" id="linkID">Home</a>

$('#linkID').prop('href'); // returns "index.html"
$('#linkID').prop('name'); // returns "linkName"
$('#linkID').prop('id'); // returns "linkID"
$('#linkID').prop('className'); // returns "link"
```

正如我们所看到的，所有在 HTML 里的 property，我们都可以通过 `prop` 方法来获取，同样的，我们还可以更新 property 通过这个方法

```javascript
$('#linkID').prop('href', 'home.html');
$('#linkID').prop('href'); // returns "home.html"
```

## What is a attribute

attribute 并不一定只包含标签默认的属性，还可能包含一些用户自己定义的属性，它与 property 主要有下面三处不同

- 用 `prop` 方法可以拿到的属性值，用 `attr` 方法一定可以拿到，但 `attr` 方法可以拿到的属性值，`prop` 方法却不一定可以拿到

    ```javascript
    <a href="index.html" id="linkID" data-tips="home">Home</a>
    
    $('#linkID').prop('href'); // returns "index.html"
    $('#linkID').attr('href'); // returns "index.html"
    
    $('#linkID').prop('data-tips'); // returns undefined
    $('#linkID').attr('data-tips'); // returns "home"
    ```

- attribute 的值永远是一个字符串，但 property 却可以是其他类型

    ```javascript
    <input type="checkbox" checked=true />
    
    $('input').prop('checked'); // return true
    $('input').attr('checked'); // return "checked"
    ```

- 如果标签的属性值有默认值，那么 attribute 显示的永远是默认值，尽管它可能已经被改了

    ```javascript
    <input type="text" name="username" vlaue="Jason" />
    
    
    $('input').prop('value', 'Liao');
    $('input').prop('value'); // return "Liao"
    $('input').attr('value'); // return "Jason"
    ```
    
    这说明了 attribute 在 HTML 文本中，property 则在 DOM Tree 中
    
## Note

还应该注意的是，不管是使用 `prop()` 还是 `attr()` 动态更新属性，另一个方法都是无法获取到这个属性的值

```javascript
<input type="text">

$('input').attr('customAttribute', 'something custom');
$('input').attr('customAttribute'); // returns "something custom"
$('input').prop('customAttribute'); // returns undefined
```

```javascript
<input type="text">

$('input').prop('customAttribute', 'something custom');
$('input').prop('customAttribute'); // returns "something custom"
$('input').attr('customAttribute'); // returns undefined
```