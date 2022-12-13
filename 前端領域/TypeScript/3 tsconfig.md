[[TypeScript]]

# Module Resolution
> [TypeScrip Module Resolution](https://www.typescriptlang.org/tsconfig#esModuleInterop)

定義模組的解析策略。預設的 `moduleResolution` 的設定為 `classic`。

例如要引入 webpack 時，可能會這麼撰寫：
```ts
import webpack from 'webpack';
```

在預設的情況下，會有「找不到模組 'webpack'。您是要將 'moduleResolution' 選項設為 'node'，或是要將別名新增至 'paths' 選項嗎？」的錯誤，這時可以將 `moduleResolution` 設定為 `node`，讓解析模組的時候可以使用 Node.js’ CommonJS 方式。

# ES Module Interop
> [TypeScript ES Module Interop](https://www.typescriptlang.org/tsconfig#esModuleInterop)

讓 TypeScript 面對 CommonJS/AMD/UMD 模組的時候，可以使用類似 ESM 的處理方式。

對應的屬性設定為 `esModuleInterop`。

# Allow Synthetic Default Imports
> [TypeScript Allow Synthetic Default Imports](https://www.typescriptlang.org/tsconfig#allowSyntheticDefaultImports)

當模組 **沒有預設的回傳值** 時，在引入模組的時候需要透過：
```ts
import * as webpack from 'webpack';
```

在 CommonJS 和 ESM 的 **預設** 回傳值分別為：
```ts
// CommonJs
module.exports.default = allFunctions;
// ESM
export default {}
```

於是，可以將 `allowSyntheticDefaultImports` 的值設定為 `true`，就可以這麼撰寫：
```ts
import webpack from 'webpack';
```

# 絕對路徑
1. `tsconfig.json` 中新增 `baseUrl` 和 `paths`。
	```json
	{
		"baseUrl": "./src",
	    "paths": {
	      "@/*":["*"]
	    }
	}
	```

2. 如果專案是使用 webpack 進行打包，則需在 `resolve` 新增 `alias` 的設定。
	```js
	{
		resolve: {
			alias: {
		      '@': path.resolve(__dirname, 'src'),
		}
	}
	```

3. 開發上即可使用 `import '@/utils/count'` 的方式引入其他模組。
