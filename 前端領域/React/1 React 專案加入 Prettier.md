[[React]]、[[環境設定]]

> 參考：
> - [Setting Up Your Editor | Create React App (create-react-app.dev)](https://create-react-app.dev/docs/setting-up-your-editor/)
> - [Integrating Prettier with ESLint for your create-react-app in VSCode](https://medium.com/@frontend-newbie/integrating-prettier-with-eslint-for-your-create-react-app-in-vscode-153ebe89c1a2)


如果專案是使用 create-react-app 的建置方式，且需要使用 Prettier 的自動格式化功能，則可以參考下方的內容。

# 執行步驟
1. 安裝套件。
	```bash
	yarn add eslint-config-prettier prettier
	```

2. 新增 `.prettierrc.json` 檔案。
	```json
	{
	  "trailingComma": "none",
	  "tabWidth": 2,
	  "semi": false,
	  "singleQuote": true,
	  "endOfLine": "auto"
	}
	```

3. 設定 `.vscode/settings.json`。
	```json
	{
	  "[typescriptreact]": {
	    "editor.defaultFormatter": "esbenp.prettier-vscode",
	    "editor.formatOnSave": true
	  }
	}
	```

4. 在 `package.json` 的 `eslintConfig` 加入 `prettier` 的擴展。
	```json
	  "eslintConfig": {
	    "extends": [
	      "react-app",
	      "react-app/jest",
	      "prettier"
	    ]
	  },
	```
	
5. 如果專案在 commit 時需要自動格式化修改的檔案，則可以參考 [Setting Up Your Editor | Create React App (create-react-app.dev)](https://create-react-app.dev/docs/setting-up-your-editor/#formatting-code-automatically)。

# 補充
因為 create-react-app 在初始建置專案時，已經自動設定 ESLint 相關的規則，這邊就不用再對 ESLint 做額外的定義。
如果要擴展或取代預設的 ESLint 設定，可以參考 [Setting Up Your Editor | Create React App (create-react-app.dev)](https://create-react-app.dev/docs/setting-up-your-editor/#extending-or-replacing-the-default-eslint-config)。

