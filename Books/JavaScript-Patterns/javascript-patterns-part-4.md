# JavaScript Patterns - Part 4

## Creational Design Patterns
.
- [Singleton Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/singleton-pattern.md)
- [Factory Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/factory-pattern.md)
- [Modual Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-Patterns/javascript-patterns-part-3.md#module-pattern)
- [Prototype Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md)

## Structural Design Patterns

- Decorator Pattern

    装饰者模式提供比继承更弹性的替代方案，他可以给我们的对象动态的添加或者修改一个行为
    
    我们常常使用到装饰者模式是当我们发现原来这个对象的方法或方法实现不满足我们要求的时候，我们可能去对这个对象进行装饰
    
    ```javascript
    // 例如我们有一个类，类中有一个 `decorate` 的方法
    var tree = {
        decorate: function () {
            console.log('Make the tree pretty');
        }
    };
    
    // 这时我们要定义我们的装饰类，重写原来类的 `decorate` 方法
    tree.RedBalls = function () {
        this.decorate = function () {
            // `this` 指的是 `RedBalls` 这个构造类
            // 当然我们执行 `getDecorator` 方法的时候
            // 这个类的原型就指向调用者，所以我们可以拿到父类，使用父类变量或者方法
            var uber = this.RedBalls.prototype;
            uber.decorate();
            console.log('Put on some red balls');
        };
    };
    
    tree.BlueBalls = function () {
        this.decorate = function () {
            var uber = this.BlueBalls.prototype;
            uber.decorate();
            console.log('Add blue balls');
        };
    };
    
    tree.Angel = function () {
        this.decorate = function () {
            var uber = this.Angel.prototype;
            uber.decorate();
            console.log('An angel on the top');
        };
    };
    
    // 想要 `tree` 可以用这些装饰类，我们还需要一个方法
    tree.getDecorator = function (deco) {
        // 把装饰类的原型指向 `tree` 类
        tree[deco].prototype = this;
        // 返回装饰后的 `tree` 类
        return new tree[deco];
    };
    
    tree = tree.getDecorator('BlueBalls');
    tree = tree.getDecorator('Angel');
    tree = tree.getDecorator('RedBalls');
    tree.decorate();
    /* Make sure the tree won't fall
       Add blue balls
       An angel on the top
       Put on some red balls
    */
    ```

- Facade Pattern

    外观模式是一个简单的模式，它只是提供一个替代的接口给对象。把尽可能简单的接口给其他的开发者，把真正复杂的实现隐藏起来，只可以使你的门面方法简短，更加的专注
    
    例如很简单的，我们一般把我们对浏览器的兼容操作都重新包起来，形成一个函数
    
    ```javascript
    var addListener = function (event, target, handler) {
        if (document.addEventListener) {
            target.addEventListener(event, handler, false);
        } else if (document.attachEvent) {
            target.attachEvent('on' + event, handler);
        } else {
            target['on' + event] = handler;
        }
    };
    ```
    
    对这种函数的优化可以看看 [这里](https://github.com/L-movingon/prepare-for-interview/blob/master/Books/JavaScript-Patterns/javascript-patterns-part-5.md#events)

- Proxy Pattern

    代理模式是以一个对象充当另一个对象的接口，它介于对象的客户端和对象本身之间，并且对该对象的访问进行保护
    
    这种模式看起来可能是额外的开销，俣是出于性能因素的考虑，它却非常有用。它可以守护我们的本体对象，并且试图使本体对象做尽可能少的工作
    
    例如这种模式的其中一个例子就是 **延迟初始化**，首先客户端发出一个初始化请求，然后代理以一切正常作为响应，但实际上却并没有将该消息传递到本体对象，直到客户端明显需要本体对象完成一些工作的时候，代理才将两个消息一起传递

- [Mixin Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/prototypal-inheritance.md#inheriting-from-multiple-prototypes)

## Behavioral Design Patterns

- Iterator Pattern

    迭代器模式其实就是一个提供迭代方法的对象，如 `next()` `hasNext()` `current()` `rewind()` 之类的方法，而常常是用模块模式来构造这个对象，数据则作为私有属性存储起来
    
    ```javascript
    var agg = (function () {
        var index = 0,
            data = [1, 2, 3, 4, 5],
            length = data.length;
        
        return {
            next: function () {},
            hasNext: function () {},
            rewind: function () {},
            current: function () {}
        };
    });
    ```
    
- [Observer Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/observer-pattern.md)
- [Mediator Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/mediator-pattern.md)
- [Strategy Pattern](https://github.com/L-movingon/prepare-for-interview/blob/master/JavaScript/strategy-pattern.md)
- Command Pattern

    命名模式的目标是将方法的调用，请求或者操作封装到一个单独的对象中，给我们酌情执行同时参数化和传递方法调用的能力
    
    实现明智简单的命令对象,将一个行为和对象对调用这个行为的需求都绑定到了一起，它们始终都包含一个执行操作，比如 `run()` 或 `execute()`
 
    ```javascript
    var CarManager = {
        requestInfo: function (model, id) {
            return "The information for " + model + " with ID " + id + " is foobar";
        },
        
        buyVehicle: function (model, id) {
            return "You have successfully purchased Item " + id + ", a " + model;
        },
        
        arrangeViewing: function (model, id) {
            return "You have successfully booked a viewing of " + model + " ( " + id + " ) ";
        }
    };
    ```
    
    当这些核心的 API 改变的时候，那么直接调用这些方法的对象和或方法也要可能跟着被修改，这可以看成是一种耦合
    
    而命令模式则是想用一个方法去统一调用这些方法，像这样
    
    ```javascript
    CarManager.execute( "buyVehicle", "Ford Escort", "453543" );
    ```
    
    所以我们定义一个 `execute` 方法
    
    ```javascript
    CarManager.execute = function ( name ) {
        return CarManager[name] && 
            CarManager[name].apply(CarManager, [].slice.call(arguments, 1));
    };
    ```