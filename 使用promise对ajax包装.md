## 使用promise对ajax进行包装

```

function ajax(url, method='get', data={}){
	return new Promise((resolve, reject) => {
		let xhr;
		if(window.XMLHttpRequest){
			xhr = new XMLHttpRequest();
		} else if(window.ActiveXObject){
			xhr = new ActiveXObject("MicroSoft.XMLHTTP");
		}
		if(!xhr) throw new Error("浏览器不支持XMLHTTPRequest和ActiveXObject");
		var params = getStringParam(data)
		// 拼接url
		url = url.indexOf("?") > -1? url + "&" + encodeURI(params): url + "?" + encodeURI(params)
		xhr.open(method, url, true);
		xhr.onload = function(){
			var result = {
				status: xhr.status,
				data: xhr.response || xhr.responseText
			}
			if(xhr.status >= 200 && xhr.status < 300 || xhr.status === 304){
				resolve(result);
			} else {
				reject(result);
			}
		}
		xhr.onerror = function(){
			reject(new Error("请求出错"));
		}
		xhr.timeout = function(){
			reject(new Error("请求超时"));
		}
		if(method === 'post'){
			xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded")
			xhr.send(params)
		}else{
			xhr.send();
		}
	})
}

function getStringParam(param){
	let dataString = ''
	for(let key in param){
		dataString += `${key}=${param[key]}&`
	}
	return dataString.slice(0, dataString.length-1);
}
```