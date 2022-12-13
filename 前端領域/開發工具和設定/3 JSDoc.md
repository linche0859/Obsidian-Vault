[[JavaScript]]、[[環境設定]]

# 設定檔
jsconfig.json
```json
{
  "include": ["./src/**/*"],
  "exclude":["./node_modules","**/node_modules/*"],
  "compilerOptions": {
    "checkJs": true,
    "downlevelIteration": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "lib": ["es2021", "dom"]
  },
}
```

.vscode/setting.json
```json
{
  "js/ts.implicitProjectConfig.checkJs": true
}
```

# 常用的 Block Tags
> 參考 [JSDoc 中文檔 - @type](https://www.shouce.ren/api/view/a/13305)。

Tag `@type` 的文件中，包含多種描述的格式，如 Multiple types、Optional parameter、Callbacks、Type definitions 等常用的格式，撰寫時可以到其中查看。