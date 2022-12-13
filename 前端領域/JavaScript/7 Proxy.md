[[JavaScript]]

> 參考 [[頂級網站技術長高度：前端工程進階大師指南]] 的第十二章。

# Proxy 和 Object.defineProperty 的差異
> 書中有 Proxy 監聽物件和使用 `Object.defineProperty` 監聽原始型別的範例，可以作為參考。

- `Object.defineProperty` 不能監聽陣列的變化，需要對陣列方法進行重新設定。
- `Object.defineProperty` 必須檢查物件的每個屬性，且需要對巢狀的物件結構進行深度檢查。
- Proxy 的代理是針對整個物件，非對特定的物件屬性，因此不用像 `Object.defineProperty` 必須檢查物件的每個屬性，Proxy 只需要做一層代理就可以監聽同級結構下的所有屬性變化，**但對於深層結構，仍需遞迴的檢查**。
- Proxy 支援代理陣列的變化。
- Proxy 的第二個參數除了可以使用 get 和 set，還可以使用 13 種攔截方法，如：將 Person 函數預設處理成非建構函數呼叫的函數，把對 Person 函數的呼叫強制轉為用 new 呼叫：
	```js
	class Person {
		constructor (name) {
			this.name = name;
		}
	}
	const proxyPersonClass = new Proxy(Person, {
		apply(target, context, args) {
			return new (target.bind(context, ...args))();
		}
	})

	proxyPersonClass('lucas'); // Person {name: 'lucas'}
	```
- 使用 Proxy 時，效能將被底層持續最佳化。

