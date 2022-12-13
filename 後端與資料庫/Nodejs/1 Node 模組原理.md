[[Node.js]]

# exports 模組設計
在 Node.js 中使用 CommonJS 將模組的內容回傳除了，可以透過 `exports.module` 的方式，也可以使用 `exports.變數` 將資料個別傳出。

```js
exports.data = 1;
exports.bark = function() {
	return 'bark;'
}

// 等於

exports.module = {
	data: 1,
	bark: function() {
		return 'bark';
	}
}
```

⚠ 注意：`exports.module` 和 `exports.變數` 是不能混用的。

# Node.js 核心模組 - createServer
```js
var http = require("http");
http.createServer(function(request, response) {
	// 需依照回傳的內容格式，給予適當的 MIME 類型
	// 更多的 MIME 類型可以參考 https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Basics_of_HTTP/MIME_types
	response.writeHead(200, {"Content-Type": "text/plain"})
	response.write("Hello")
	response.end()
}).listen(8080);
```

## 常見的通訊埠 (port)
- 21 - FTP
- 80 - Http
- 3389 - 遠端桌面

# Node 模組 - Path
當載入 path 模組後，便可使用它的語法取得檔案與目錄路徑，詳細可以瀏覽 [Path | Node.js v17.5.0 Documentation (nodejs.org)](https://nodejs.org/api/path.html) 文件。