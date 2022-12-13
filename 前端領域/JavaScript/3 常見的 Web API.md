[[JavaScript]]

> 參考：[[頂級網站技術長高度：前端工程進階大師指南]] 的第三章。

# getBoundingClientRect 方法
要取得元素距離視窗邊界的方法，可以參考 3-4 頁的方法。

補充：
- ownerDocument 為文件，即為 `<!DocType>`，documentElement 為根節點。
	可以參考：[Node.ownerDocument - Web APIs | MDN (mozilla.org)](https://developer.mozilla.org/zh-TW/docs/Web/API/Node/ownerDocument)
- clientTop 表示元素頂部外框的寬度，不包含 margin 和 padding。

# Reduce 方法
## 透過 reduce 實現依序執行 promise
```js
const runPromiseInSequence = (array, value) =>
  array.reduce((chain, current) => {
    // console.log('chain', chain)
    // console.log('current', current)
    return chain.then(current)
  }, Promise.resolve(value))

const f1 = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('f1 running')
      resolve('f1 end')
    }, 500)
  })

const f2 = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('f2 running')
      resolve('f2 end')
    }, 500)
  })

run([f1, f2], 'init')
```

當初在 `runPromiseInSequence` 中卡住了一段時間，不懂為什麼 `f2` 函數會被執行，原因為 `Promise.then` 傳入的函數為 callback function，因此會被執行。

## compose 方法
### Redux 實現 compose 方法
```js
const compose = (...funcs) => {
  if (funcs.length === 0) return arg => arg
  if (funcs.length === 1) return funcs[0]

  return funcs.reduce((acc, cur) => {
    console.log('acc', acc)
    console.log('cur', cur)
    return (...args) => acc(cur(...args))
  })
}

const f3 = val => {
  console.log('f3', val)
  return ++val
}

const f4 = val => {
  console.log('f4', val)
  return ++val
}

const f5 = val => {
  console.log('f5', val)
  return ++val
}

compose(f5, f4, f3)(1)

// result
// f3 1
// f4 2
// f5 3
// 4
```

最後一個回傳 `4` 的原因為 f5 函數也有回傳值。

### 結合 Promise 的依序執行
```js
const composePromise = (...funcs) => {
  const init = funcs.pop()
  return (...args) => {
    funcs.reverse().reduce((acc, cur) => acc.then(result => cur(result)), Promise.resolve(init.apply(null, args)))
  }
}
```

