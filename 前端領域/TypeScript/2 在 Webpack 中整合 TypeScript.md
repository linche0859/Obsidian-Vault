[[TypeScript]]

可以參考 Webpack 的官方文件 ([TypeScript | webpack](https://webpack.js.org/guides/typescript/)) 進行安裝和設定。

其中，紀錄在整合時發現的疑慮。

# Loader
Webpack 官方推薦使用 `ts-loader` 進行編譯，但如果在原來的專案中已經有使用 Babel 做編譯的轉換，建議改成安裝 [@babel/preset-typescript](https://babeljs.io/docs/en/babel-preset-typescript)，取代安裝 `ts-loader`。

## @babel/preset-typescript
安裝和設定的步驟如下：
1. 安裝 `@babel/preset-typescript`。
	```shell
	npm install --save-dev @babel/preset-typescript
	```

2. `babel.config.json` 檔案中調整 `presets`。
	```json
	{
	  "presets": ["@babel/preset-typescript"]
	}
	```

3. `webpack.config.js` 檔案中調整需要驗證的副檔名。
	```js
	module.exports = {
		module: {
			rules: [
			   {
		         test: /\.m?(js|ts)$/,
		         exclude: /node_modules/,
		         use: {
		           loader: 'babel-loader',
		         },
		       },
			]
		}
	}
	```

4. 如果需要編譯後，可以支援到某個瀏覽器的版本，可以在 `.browserslistrc` 檔案中設定支援的版本。
	```
	last 2 versions
	```
	
	也就是 babel 的最後編譯結果，會由 `.browserslistrc` 檔案的內容來決定。

## ts-loader
`ts-loader` 會使用 `tsconfig.json` 的設定值決定輸出的版本，例如設定檔的 `target` 為 `es5`，那麼編譯後的結果就會轉為 `es5` 的版本。

### 執行過程
> 參考 [ts-loader](https://webpack.js.org/guides/typescript/#loader) 文件。

1. 透過 tsc 將 TypeScript 轉換成 JavaScript。
2. 依據 `tsconfig.json` 的 `module` 設定，決定編譯的模組。
	`module` 的設定可以參考：[TypeScript Module](https://www.typescriptlang.org/tsconfig#module)
	⚠ 如果將 `module` 設定為 `CommonJS`，則會失去 Tree Shaking 的功能。


