[[Vue]]

> 參考：[使用 Vue3 開發 Web Component 入門](https://medium.com/i-am-mike/%E4%BD%BF%E7%94%A8-vue3-%E9%96%8B%E7%99%BC-web-component-%E5%85%A5%E9%96%80-1552f8563bc0)

# 環境準備
1. 使用 [Vite](https://vitejs.dev/guide/#trying-vite-online) 建置專案。
2. 參考 Vite 的 [Build for Production](https://vitejs.dev/guide/build.html#library-mode) 設定打包的入口，Vue 的 [Web Component](https://vuejs.org/guide/extras/web-components.html#using-custom-elements-in-vue) 定義 Custom Component。
    
3. 設定 `vite.config.js`。
    ```js
    import { defineConfig } from 'vite';
    import vue from '@vitejs/plugin-vue';
    import path from 'path';
    
    // https://vitejs.dev/config/
    export default defineConfig({
      plugins: [
        vue({
          template: {
            compilerOptions: {
              // treat all tags with a dash as custom elements
              isCustomElement: (tag) => tag.includes('-'),
            },
          },
        }),
      ],
      build: {
        lib: {
          entry: path.resolve(__dirname, 'src/custom/main.js'),
          name: 'MyCustom',
          fileName: (format) => `my-custom.${format}.js`,
        },
      },
    });
    ```

    💡 `vite.config.js` 中 `build` 不使用 `rollupOptions` 的原因：使用打包後的 Web Component 位置，也需載入和開發 Web Component 一樣的 Vue 環境 (版本)，當移除 `rollupOptions` 後，會將目前開發的 Vue Module 一同打包和輸出，雖然最後的 Bundle Size 會較大 (因為包含整個 Vue Module)，但能確保 Web Component 可以正常運作。
    
4. 定義 Custom Component 的主要入口。
    ```js
    // /src/customs/main.js
    import { defineCustomElement } from 'vue';
    import Example from './Example.ce.vue';
    
    // convert into custom element constructor
    const ExampleElement = defineCustomElement(Example);
    
    // register
    customElements.define('my-example', ExampleElement);
    ```

# Build Production
使用 `vite build` 指令打包成上線的版本，可以觀察到 `dist` 資料夾下新增了兩個檔案：

- my-custom.es.js
- my-custom.umd.js

如果想要測試，可以新增一個 HTML 檔案，並引入 `my-custom.umd.js` 的 script，再至 HTML 的 `<body>` 中加上：
```html
<my-example index="4"></my-example>
```

就可以成功看到 Web Component 的顯示囉。

# 結論
過去要將相同的內容重複的利用，可能會透過 Copy & Paste、IFrame 等，在 IE 漸漸結束的現代，可以使用 Web Component 更有效地達成功能，使用 Web Component 的好處有：

1. 解決了維護的議題。
2. 不受技術的限制 (不論要使用 React、Vue 等技術都可以)。
3. 沒有干擾議題：Global Style 不會干擾 Shadow Component 的樣式，雖然仍可以做到跨層的影響，但通常不會這麼做。