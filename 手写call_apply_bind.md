## 实现call

1.判断当前this是否为函数，防止 Function.prototype.myCall() 直接调用
2.context 为可选参数，如果不传的话默认上下文为 window
3.为context 创建一个 Symbol（保证不会重名）属性，将当前函数赋值给这个属性
4.处理参数，传入第一个参数后的其余参数
5.调用函数后即删除该Symbol属性
6.返回执行结果

```

	Function.prototype.myCall = function (context = window, ...args) {
		if (Function.prototype === this) {
			return undefined; // 防止Function.prototype.myCall()自己调用
		}
		context = context || window;
		const fn = Symbol();
		context[fn] = this;  // this指向myCall前面的函数
		const result = context[fn]();
		delete context[fn];
		return result;
	}
```

## 实现apply

- 过程与call类似，只是接收参数为数组

```

	Function.prototype.myApply = function (context = window, args) {
		if (Function.prototype === this) {
			return undefined; // 防止Function.prototype.myCall()自己调用
		}
		context = context || window;
		const fn = Symbol();
		context[fn] = this;  // this指向myApply前面的函数
		var result;
		if (Array.isArray(args)) {
			result = context[fn](...args);
		} else {
			result = context[fn]();
		}
		delete context[fn];
		return result;
	}
``` 

## 实现bind

- 与call、apply不同的是，bind并不会立即执行，而是后续函数执行时才调用，接收参数为数组

```

	Function.prototype.myBind = function (context, ...args1) {
		if (this === Function.prototype) {
			throw new TypeError('Error');
		}
		const _this = this;
		return function F(...args2) {
			return _this.apply(context, args1.concat(args2));
		}
	}
```
