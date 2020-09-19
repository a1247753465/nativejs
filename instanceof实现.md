## instanceof实现

- **a instanceof Object**的原理是判断Object的原型(prototype)是否在a的原型链(__proto__)上

```

	function myInstanceof(target, origin) {
		const proto = target.__proto__;
		if(proto){
			if(origin.prototype === proto){
				return true;
			} else {
				myInstanceof(proto, origin);
			}
		} else {
			return false;
		}
	}
```