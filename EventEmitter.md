## 手写EventEmitter

```

	function EventEmitter(){
	  this._maxListeners = 10;
	  this._events = Object.create(null)
	}
	
	EventEmitter.prototype.addEventListener = function (type, listener, prepend) {
	  if(!this._events) {
	    this._events = Object.create(null)
	  }
	  if(this._events[type]) {
	    if(prepend) {
	      this._events[type].unshift(listener)
	    }else{
	      this._events[type].push(listener)
	    }
	  }else{
	    this._events[type] = [listener]
	  }
	}
	
	EventEmitter.prototype.emit = function(type, ...args) {
	  if(Array.isArray(this._events[type])) {
	    this._events[type].forEach(fn => {
	      fn.apply(this, args)
	    })
	  }
	}
	
	EventEmitter.prototype.removeListener = function(type, listener) {
	  if(Array.isArray(this._events[type])){
	    if(!listener) {
	      delete this_events[type]
	    }else{
	      this._events[type] = this._events[type].filter(e => e!==listener && e.origin !== listener)
	    }
	  }
	}
	
	EventEmitter.prototype.once = function(type, listener) {
	  const only = (...args) => {
	    listener.apply(this, args);
	    this._events[type].removeListener(type, listener)
	  }
	  only.origin = listener
	  this.addEventListener(type, only)
	}
```