[[JavaScript]]

> 參考 [[頂級網站技術長高度：前端工程進階大師指南]] 的第二十二章。

# husky
透過 Git 指令的鉤子，在 Git 指令進行到某一個時段時，可以完成某些特定的操作。

## 安裝和設定
1. 安裝 `husky`。
	```shell
	yarn add husky
	```

2. 安裝 `lint-staged`，因為在整個專案上執行 lint 會很慢，基本只會對更改的檔案進行檢查，這時就需要用到 lint-staged。
	```bash
	yarn add lint-staged
	```

3. `package.json` 檔案中增加 `husky` 和 `lint-staged` 內容：
	```json
	{
		"scripts": {
			"lint": "eslint --ext ./src/**/*.js",
			"lint:write": "eslint --ext ./src/**/*.js --fix",
			"lint:style": "stylelint ./src/**/*.css",
			"format": "prettier --write \"**/*.{js,css}\""
		},
		"lint-staged": {
			"*.(js|jsx)": ["npm run lint:write", "npm run format", "git add"],
			"*.css": ["npm run lint:style", "npm run format", "git add"]
		},
		"husky": {
			"hooks": {
				"pre-commit": "lint-staged"
			}
		}
	}
	```

