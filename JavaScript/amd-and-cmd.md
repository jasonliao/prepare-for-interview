# AMD and CMD

- Asynchronous Module Definition - 是 RequireJS 在推广过程中对模块定义的规范化产出
- Common Module Definition - 是 SeaJS 在推广过程中对模块定义的规范化产出

## CommonJS

一开始 CommonJS 在 Node.js 等服务器环境下取得了很不错的实践。

根据 CommonJS 的规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，一个文件定义的变量对于其他文件都是私有的。

如果想要多个文件共享一个变量，就要定义为 global 对象的属性

然后通过 `module.exports` 这个对象，可以被其他的文件通过 `require` 进行导入。

## AMD & RequireJS

CommonJS 在服务器端的成功，让社区里的人想把规范进一步推广到浏览器上。但是却在定义规范的时候，产生了分歧。慢慢地形成了三个流派。

其中一个就是 AMD 规范及其实现的 RequireJS

AMD 推崇的是提前执行，把所有依赖的模块先异步加载，模块的加载并不影响后面语句的执行，而所有依赖这些模块的语句，就会放到一个回调函数里面，等到加载完之后，这个回调函数才会执行。

例如采用 AMD 的规范来定义一个模块 `math.js`

```javascript
// math.js

define(function () {
	var add = function (x, y) {
		return x + y;
	};
	return {
		add: add
	}
});
```

那么我们就可以在其他地方加载这个模块，例如在 `main.js` 里

```javascript
// main.js

require(['math'], function (math) {
	console.log(math.add(1, 1));
});
```

而如果在定义模块的时候，还会用到其他模块的时候，那么 `define` 的第一个参数就是一个数组，要声明要加载的模块

```javascript
define(['myLib'], function (myLib) {
	// dosomething with myLib
});
```

## CMD & SeaJS

CMD 推崇的是依赖就近，当依赖真正使用的时候，才去加载这个模块并执行。

如果使用 CMD 的规范去定义一个模块，则可以等到真正需要用到另一个模块的时候，就才加载执行。

```javascript
define(function (require, exports, module) {
	function bar () {
		// if need another module
		require('myLib');
		// dosomething with myLib
	}
	exports.bar = bar;
});
```

## Difference between RequireJS and SeaJS

1. 定位有差异

	RequireJS 想成为浏览器端的模块加载器，同时也想成为 Rhino / Node 等环境的模块加载器。SeaJS 则专注于 Web 浏览器端，同时通过 Node 扩展的方式可以很方便跑在 Node 环境中。

2. 遵循的规范不同

	RequireJS 遵循 AMD（异步模块定义）规范，SeaJS 遵循 CMD （通用模块定义）规范。规范的不同，导致了两者 API 不同。SeaJS 更贴近 CommonJS Modules/1.1 和 Node Modules 规范。

3. 推广理念有差异

	RequireJS 在尝试让第三方类库修改自身来支持 RequireJS，目前只有少数社区采纳。SeaJS 不强推，采用自主封装的方式来“海纳百川”，目前已有较成熟的封装策略。

4. 对开发调试的支持有差异

	SeaJS 非常关注代码的开发调试，有 nocache、debug 等用于调试的插件。RequireJS 无这方面的明显支持。

5. 插件机制不同

	RequireJS 采取的是在源码中预留接口的形式，插件类型比较单一。SeaJS 采取的是通用事件机制，插件类型更丰富。

## Reference

- [前端模块化开发那点历史](https://github.com/seajs/seajs/issues/588)
- [SeaJS与RequireJS的异同](https://github.com/seajs/seajs/issues/277)
