---
categories: [前端, JS]
tags: [websocket, JS]
---

html代码
```html
<body>
	<input type="text" id="input">
	<button onclick="send()">发送</button>
	<div id="box"></div>
	<script>
		const inputDom = document.getElementById('input');
		const boxDom = document.getElementById('box');
		const websocket = new WebSocket('ws://localhost:8080');
		websocket.onmessage = function (message) {
		boxDom.innerText += message.data;
	}
	function send() {
		console.log('send message:' + input.value);
		websocket.send(inputDom.value);
	}
	</script>
</body>
```

node服务器代码
```javascript
const { WebSocketServer } = require('ws');
const wss = new WebSocketServer({port: 8080});
wss.on('connection', function (ws) {
	ws.on('open', () => {
		ws.send('The connection is open');
	})
	ws.on('message', function (message) {
		console.log(message.toString());
		ws.send(message.toString());
	});
	ws.on('close', function () {
		console.log('The connection is close');
	});
	ws.on('error', function (err) {
		console.log(err);
	});
});
```
