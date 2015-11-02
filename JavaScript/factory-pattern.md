# Factory Pattern

工厂模式并不需要我们用构造函数去创建我们的对象，我们只需要调用一个工厂的方法，并把我们想要的类型传进去，工厂就会帮我们创建这个对象并返回

例如我们有一个 UI 工厂，可以给我们创建不同类型的 UI 组件，这时我们只要调用工厂方法，并把我们想要的 UI 组件传进去，如 `Button` `Panel`，这样就不需要去直接用 `new` 的方式调用不同组件的构造函数

当我们创建对象过程相对复杂的时候，这个模式就非常有用，这种模式在例如 ExtJS 这样的 UI 库里可以找到

我们先来看一个造车的例子

```javascript
var vehicleFactory = vehicleFactory || {}; // 定义一个车工厂对象

// 虽然我们不直接调用构造函数，但我们还是要有
vehicleFactory.Car = function (options) {
    this.door = options.doors || 4;
    this.state = options.state || 'brand new';
    this.color = options.color || 'silver';
}
vehicleFactory.Truck = function (options) {
    this.state = options.state || 'used';
    this.wheelSize = options.wheelSize || 'large';
    this.color = options.color || 'blue';
}

vehicleFactory.vehicleClass = Car; // 让工厂默认造 Car 这种类型
vehicleFactory.createVehicle = function (options) {
    // 根据传进来的 `options.vehicleType` 来改变工厂造的车的类型
    switch (options.vehicleType) {
        case 'car':
          this.vehicleClass = this.Car;
          break;
        case 'truck':
          this.vehicleClass = this.Truck;
          break;
    }
    return new this.vehicleClass(options); // 返回实例
};

var car = vehicleFactory.createVehicle({
    vehicleType: 'car',
    color: 'yellow',
    doors: 6
}); // 用工厂造了一辆车

console.log(car); // { vehicleType: 'car', color: 'yellow', doors: 6 }
console.log(car instanceof vehicleFactory.Car); // true
```

但我们想要造一辆卡车的时候，只要传入对象的 `vehicleType` 为 `truck` 即可

```javascript
var movingTruck = vehicleFactory.createVehicle({
    vehicleType: 'truck',
    state: 'like new',
    color: 'red',
    wheelSize: 'small' 
});
```

我们还可以创建一个子工厂，只生产固定种类的车

```javascript
var truckFactory = Object.create(vehicleFactory); // 继承于大工厂
truckFactory.vehicleClass = vehicleFactory.Truck; // 改变默认车型

var myBigTruck = truckFactory.createVehicle({
    state: "omg..so bad.",
    color: "pink",
    wheelSize: "so big" 
}); // 当没有传入 `vehicleType` 的时候，造默认车型 Truck
```

## When To Use The Factory Pattern

- 对象或组件的构建十分复杂
- 需要依赖具体环境创建不同实例
- 处理大量具有相同属性的小对象或组件

## When Not To Use The Factory Pattern

除非需要为创建比较复杂的对象提供接口，否则，不要滥用运用工厂模式，有时候仅仅只是给代码增加了不必要的复杂度，同时使得测试难以运行下去

## Build-in Object Factory

`Object()` 构造函数就是我们一个内置的工厂，它可以根据你们输入的类型来创建不同的对象，例如你输入一个原始数字，那么它就能够在后台以 `Number()` 的构造函数创建一个对象

`Object()` 无论使用 `new` 操作符与否，都可以调用 `Object()`

## Why We Have Factory Pattern

正如上面所说，当我们对象或组件的构建十分复杂的时候，我们就会用到工厂模式，那为什么我们就不直接把所有的构造函数开放出去呢？因为这样就会造成非常大的耦合度。在面向对象的思想中，我们是想类做到高内聚，低耦合，所以我们的构造类只提供了一个工厂接口，你想创建什么类型的实例，我就先在内部创建好了，再返回给你