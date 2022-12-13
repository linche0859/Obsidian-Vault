[[Vite]]、[[環境設定]]

# 建置步驟
1. 建立專案。
	```bash
	npm create vite@latest 你的專案名稱
	```

2. 安裝 ESLint 的套件和建立設定檔。
	```bash
	eslint --init
	或
	npm init @eslint/config
	```

	預設建立的設定檔 (`.eslintrc.cjs`) 可能會有 `rules` 的規則，可以都先移除。

3. 安裝 Prettier。
	```bash
	npm install prettier eslint-config-prettier -D
	```

	`eslint-config-prettier` 套件主要用於取消原本的 ESLint 設定，改由 Prettier 接手處理程式的驗證。

4. 將 `eslint-config-prettier` 的啟用規則設定於 ESLint 的設定檔中。
	完成的設定檔會如下：
	```js
	module.exports = {
	  env: {
	    browser: true,
	    es2021: true
	  },
	  extends: [
	    'eslint:recommended',
	    'plugin:react/recommended',
	    'plugin:@typescript-eslint/recommended',
	    'eslint-config-prettier'
	  ],
	  overrides: [],
	  parser: '@typescript-eslint/parser',
	  parserOptions: {
	    ecmaVersion: 'latest',
	    sourceType: 'module'
	  },
	  plugins: ['react', '@typescript-eslint'],
	  rules: {}
	}
	```

5. 新增 Prettier 的規則檔案 (`.prettierrc.json`)。
	```json
	{
	  "trailingComma": "none",
	  "tabWidth": 2,
	  "semi": false,
	  "singleQuote": true,
	  "printWidth": 120,
	  "bracketSpacing": true
	}
	```

6. 新增 VSCode 對 `ts` 和 `tsx` 的檔案支援儲存時的自動排版。
	```json
	{
	  "[typescriptreact]": {
	    "editor.defaultFormatter": "esbenp.prettier-vscode",
	    "editor.formatOnSave": true
	  }
	}
	```

	比較統一的方式，可以這麼設定：
	```json
	{
	  "editor.defaultFormatter": "esbenp.prettier-vscode",
	  "editor.formatOnSave": true
	}
	```