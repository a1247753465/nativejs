## promise实现

```

	function MyPromise(exeutor){
	  this._status = "pending";
	  this.value = null;
	  this.reason = null;
	  const resolve = (value) => {
	    this._status = "fulfilled";
	    this.value = value;
	  }
	  const reject = (reason) => {
	    this._status = "rejected";
	    this.reason = reason;
	  }
	  try{
	    exeutor(resolve, reject)
	  }catch(err){
	    reject(err)
	  }
	}
	
	
```

### promise.then

```

	MyPromise.prototype.then = function(onFulFilled, onRejected){
	  switch(this._status){
	    case 'fulfilled':
	      setTimeout(() => {
	        onFulFilled(this.value);
	      }, 0)
	      break;
	    case 'rejected':
	      setTimeout(() => {
	        onRejected(this.reason);
	      }, 0)
	      break;
	  }
	}
```

### promise.all

```

	MyPromise.all = function(promises){
	  return new Promise(function(resolve, reject){
	    if(!promises.length){
	      resolve([])
	    }else{
	      var result = [];
	      var count = 0;
	      for(var i=0, len=promises.length; i<len; i++){
	        promises[i].then(data => {
	          result.push(data);
	          if(++count === len){
	            resolve(result);
	          }
	        }, err => reject(err));
	      }
	    }
	  })
	}
```

### promise.race

```

	MyPromise.race = function(promises){
	  return new Promise(function(resolve, reject){
	    if(!promises.length){
	      resolve();
	    }else{
	      var count = 0;
	      for(var i=0, len=promises.length; i<len; i++){
	        promises[i].then(data => {
	          resolve(data);
	        }, err => {
	           reject(err);
	        })
	      }
	    }
	  })
	}
```