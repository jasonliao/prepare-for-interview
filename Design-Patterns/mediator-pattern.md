# Mediator Pattern


一个应用可能有多个不同的小对象组合在一起，这些小对象之间的直接交流就会导致耦合度很高，这样当我们要修改一个对象的时候，我们就会影响到其他的对象

中介者模式就是出来解决这一个问题的，一个对象先与中介者进行交互，然后中介再与另一个对象交互。从而达到解耦的目的

我们之前介绍的观察者模式和中介者模式有点相似，都是一个模式对象去控制其他对象，但不同的是，观察者模式是一个本体对象的某个动作，触发了一系列不同对象的动作，是单向的，而中介者模式则可以与不同的对象有双向的交流，目的是为了不让对象与对象之间直接交流

## How it works

例如我们写一个简单的游戏，这个游戏涉及到的对象有

- Player 1
- Player 2
- Scoreboard

我们先定义这些类

```javascript
function Player (name) {
    this.points = 0;
    this.name = name;
}

Player.prototype.play = function () {
    this.points += 1;
};
```

```javascript
var scoreboard = {
    element: document.getElementById('results'),
    update: function (Home, Guest) {
        var i,
            msg = '',
            score = {
                Home: Home.points,
                Guest: Guest.points
            };
        for (i in score) {
            if (score.hasOwnProperty(i)) {
                msg += '<p><strong>' + i + '<\/strong>: ';
                msg += score[i];
                msg += '<\/p>';
            }
        }
        this.element.innerHTML = msg;
    }
};
```

如果当我们不用中介者模式，直接两个类进行沟通

```javascript
var Player1 = new Player('Home'),
    Player2 = new Player('Guest');
    
window.onkeypress = function (e) {
    e = e || window.event;
    if (e.witch === 49) {
        Player1.play();
        scoreboard.update(Player1, Player2);
        return;
    }
    if (e.witch === 48) {
        Player2.play();
        scoreboard.update(Player1, Player2);
        return;
    }
};
```

所以我们看到，在不用中介者的时候，我们 `window` 对象要调用 `Player` 对象和 `scoreboard` 对象，`scoreboard` 对象里又要调用 `Player` 对象。这么简单的例子就有这样较大的耦合度，如果当我们使用了中介者模式后，对象之间的调用就没有了

首先创建 `mediator` 对象

```javascript
var mediator = {
    players: {},
    setup: function () {
        // 把对象的创建放在这初始化这里
        var players = this.players;
        players.home = new Player('Home');
        players.guest = new Player('Guest');
    },
    played: function () {
        // 更新得分板
        var players = this.players,
            score = {
                HOME: players.home.points,
                Guest: players.guest.points
            };
        scoreboard.update(score);
    },
    keypress: function (e) {
        e = e || window.event;
        if (e.witch === 49) {
            this.players.home.play();
            return;
        }
        if (e.witch === 48) {
            this.players.guest.play();
            return;
        }
    }
};
```

```javascript
Player.prototype.play = function () {
    this.points += 1;
    // 修改 Player 的 play 方法
    // 每当按下键得一分，就调用 `mediator` 的 `played` 方法
    // 更新得分板
    mediator.played();
};
```

```javascript
var scoreboard = {
    // ...
    update: function (score) {
        var i,
            msg = '';
        for (i in score) {
            if (score.hasOwnProperty(i)) {
                msg += '<p><strong>' + i + '<\/strong>: ';
                msg += score[i];
                msg += '<\/p>';
            }
        }
        this.element.innerHTML = msg;
    }
};
```

当我们添加了中介者之后，对象之间的交互都由中介者来处理，对象只与中介者交互

```javascript
mediator.setup();
window.onkeypress = mediator.keypress;
```

## Advantages & Disadvantages

中介者模式最大的好处就在于可以减少对象与对象，或者组件与组件之间的耦合度，但是新增的对象会带来性能的问题