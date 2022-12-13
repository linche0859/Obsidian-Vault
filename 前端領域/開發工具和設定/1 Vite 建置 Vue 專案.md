[[Vite]]、[[環境設定]]

# 環境變數
可以在專案的跟目錄下建立 `.env` 檔案，但它不會區分當前的執行環境，因此調整為建立 `.env.development` 檔案，並將變數 prefixed 加上 `VITE_` ，目的為防止將環境變數洩漏於客戶端。

> 可以參考 [Env Variables and Modes | Vite (vitejs.dev)](https://vitejs.dev/guide/env-and-mode.html#env-files)

當專案打包後，在 `production` 環境下無法由 `import.meta.env.自定義的變數` 取得環境變數，需在 `vite.config.js` 中透過 `vite.loadEnv` 將環境檔載入，才可以做使用：

```js
import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default ({ mode }) => {
  const envConfig = loadEnv(mode, './')
  return defineConfig({
    plugins: [vue()],
    base: envConfig.VITE_PUBLISH_PATH,
    resolve: {
      alias: {
        '@': path.resolve(__dirname, 'src')
      }
    }
  })
}
```

# ESLint 和 Prettier
想要透過 ESlint 規範程式碼風格，和利用 Prettier 自動排版，可以這麼做：

1.  初始化 ESLint 並安裝。
    ```
    eslint --init
    ```
    
    可以選擇 popular style (如：standard) 作為規範。
	
	- VSCode 的環境設定檔
		```json
		// setting.json
		{
		  "editor.codeActionsOnSave": {
			"source.fixAll.eslint": true
		  },
		  "eslint.validate": ["javascript", "html"],
		  "[javascript]": {
			"editor.formatOnSave": false
		  },
		}
		```
		
	-  Stylelint 的設定可以參考 [[1.1 Vite 環境使用 Tailwind CSS + SCSS]]。

2.  安裝 Prettire 相關套件：
	> 參考 [Integrating with Linters · Prettier](https://prettier.io/docs/en/integrating-with-linters.html)
    
	```bash
	yarn add eslint-config-prettier stylelint-config-prettier eslint-plugin-prettier stylelint-prettier prettier -D
	```
    
3.  將 Prettier 規範加入 ESLint 設定檔中。
    ```js
    module.exports = {
      env: {
        browser: true,
        es2021: true,
        'vue/setup-compiler-macros': true
      },
      extends: ['plugin:vue/essential', 'standard', 'plugin:prettier/recommended'],
      parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module'
      },
      plugins: ['vue'],
      rules: {
        'vue/multi-word-component-names': 'off',
        'vue/no-multiple-template-root': 'off'
      }
    }
    ```
    
    -   `env` 屬性下的 `vue/setup-compiler-macros` ：vue 檔的 `<script setup>` 可以使用全域方法，如：`defineProps` 等。
    -   `rules` 屬性下的 `vue/multi-word-component-names`：元件的名稱 (檔名) 可以是一個名詞，不用一定要用組合的方式。
    -   `rules` 屬性下的 `vue/no-multiple-template-root`：允許 `<template>` 下有多個第一層節點。

4. 新增 `.prettierrc.json`：
	```json
	{
	  "trailingComma": "none",
	  "tabWidth": 2,
	  "semi": false,
	  "singleQuote": true,
	  "endOfLine": "auto"
	}
	```

5. 設定 Git 指令的動作執行。可以參考 [[4 husky 和 lint-staged]] 的設定方式。
6. 如果需要透過指令將全部的檔案自動排版，可以在 `package.json` 的 `scripts` 中增加：
	```json
	{
	  "scripts": {
		  "format": "prettier --write \"**/*.{vue,css}\""
	  }
	```

# Vue Router 的 history
當部屬至 Github Pages 後，看到路由只會引導至 404 的頁面，而在開發時，路由的顯示又為正常，原因是在預設情形，路由會將 `/` 作為根路徑，而 Github 會依據不同的 Repository，最後的靜態檔案的路徑也會所不同，這時只要在 Vue Router 的 `history` 屬性，不論是 Hash 或 History 模式傳入路徑，就可以調整路由的 base 位置。

```js
const router = createRouter({
  history: createWebHistory(import.meta.env.VITE_PUBLISH_PATH),
  routes
})
```