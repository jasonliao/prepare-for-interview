# Observer Pattern

所有的浏览器事件 (mousemove, keypress, and so on) 其实都是这种模式的例子
    
不同于一个对象调用另一个对象的方法，这种模式采用的是有一个对象订阅了另一个对象的特殊行为，前者我们叫观察者，也叫订阅者。被观察的对象则被称为发布者或主体，当主体有状态的变化的时候，就会告诉观察者，有时还可能会把信息传给他们

先定义一个发布者对象

```javascript
var publisher = {
    subscribers: [], // 订阅者列表
    subscribe: function (fn) { // 订阅方法
        this.subscribers.push(fn);
    },
    unsubscribe: function (fn) { // 退订方法
        var subscribers = this.subscribers,
            i,
            max = subscribers.length;
        for (i = 0; i < max; i++) {
            if (subscribers[i] === fn) { // 找到传进来的方法，去掉
                subscribers.splice(i, 1);
            }
        }
    },
    publish: function (publication) {
        var subscribers = this.subscribers,
            i,
            max = subscribers.length;
        for (i = 0; i < max; i++) {
            subscribers[i](publication); // 把该类型的订阅方法都执行一次
        }
    }
};
```

定义一个普通对象，要成为一个发布者，可以继承发布者对象，或者对象组合

```javascript
var paper = Object.assign({}, publisher, {
    daily: function () {
        this.publish('" Why Jason so good? "');
    }
});
```

这时就是看谁订阅了这个 paper 的 daily，例如 Jason 和 Liao 订阅了

```javascript
var Jason = {
    readDaily: function (news) {
        console.log('Jason reading ' + news + ' in the morning');
    }
};

var Liao = {
    readDaily: function (news) {
        console.log('Liao reading ' + news + ' at night');
    }
};

paper.subscribe(Jason.readDaily);
paper.subscribe(Liao.readDaily);
```
    
这时只要 paper 每天早上都发布了新消息，那么订阅者就都知道

```javascript
paper.daily();
// Jason reading " Why Jason so good? " in the morning
// Liao reading " Why Jason so good? " at night
```

## When to Use Observer Pattern

当一个对象的改变需要同时改变其它对象，并且它不知道具体有多少对象改变的时候，就应该考虑使用观察者模式

## Advantages

观察者模式会促使我们去思考两个部分之间的联系，更有效地让我们把大应用分解成小的部分，耦合度低的代码块更有利于我们代码的重用
