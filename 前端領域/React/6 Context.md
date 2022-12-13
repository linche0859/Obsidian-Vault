[[React]]

可以想像成作用域，在這個作用域的範圍內，都可以取得特定的狀態。

# 適用情境
當畫面使用到 **多個巢狀元件，且需要將狀態不斷向下傳遞** 時，可以考慮使用 Context。

建議 **不要將 Provider 放置於多個沒有共同狀態的最上層根元件**，因為當 Provider 的 Context 狀態更新時，下方的子元件會全部觸發重新渲染，造成不必要的資料浪費。

# 問題原因
還未使用 Context 的 props 傳遞方式，需要將元件的 `btnVisible` 狀態傳遞到最裡面的 `D` 元件。
```tsx
type Props = {
  btnVisible: boolean
}

const D: React.FC<Props> = ({ btnVisible }) => {
  return (
    <>
      <p>D 組件</p>
      <button>D 按鈕</button>
    </>
  )
}

const C: React.FC<Props> = ({ btnVisible }) => {
  return (
    <>
      <p>C 組件</p>
      <D btnVisible={btnVisible} />
    </>
  )
}

const B: React.FC<Props> = ({ btnVisible }) => {
  return (
    <>
      <p>B 組件</p>
      <C btnVisible={btnVisible} />
    </>
  )
}

const Context = () => {
  const [btnVisible, setBtnVisible] = useState(true)
  return (
    <>
      <div>Context</div>
      <B btnVisible={btnVisible} />
    </>
  )
}
```

# 解決方式
1. 建立 Provider 元件和自訂義的 Context Hook。
	```tsx
	// contexts/BtnContext.tsx
	import React, { createContext, useState, useContext } from 'react'

	const defaultValue = {
	  btnVisible: false
	}
	
	const BtnContext = createContext(defaultValue)
	
	export const BtnProvider: React.FC<React.PropsWithChildren> = ({
	  children
	}) => {
	  const [btnVisible, setBtnVisible] = useState(true)
	  return (
	    <BtnContext.Provider value={{ btnVisible }}>{children}</BtnContext.Provider>
	  )
	}
	
	export const useBtnContext = () => useContext(BtnContext)
	```

	- 定義 `defaultValue` 原因 -> 當子元件沒有被 Provider 元件包裹時，預設取得會是 `defaultValue.btnVisible` 中的值 (`false`)。
	- 被 `BtnProvider` 包裹的子元件，預設會取得 `BtnProvider` 中的 `btnVisible` (`true`) 狀態。

2. 在需要使用 `btnVisible` 狀態的位置，呼叫自訂義的 Hook (`useBtnContext`)。
	```tsx
	type Props = {}

	const D: React.FC<Props> = () => {
	  const inject = useBtnContext()
	  return (
	    <>
	      <p>D 組件</p>
	      <p>D btnVisible:{inject.btnVisible.toString()}</p>
	      <button>D 按鈕</button>
	    </>
	  )
	}
	
	const C: React.FC<Props> = () => {
	  return (
	    <>
	      <p>C 組件</p>
	      <D />
	    </>
	  )
	}
	
	const B: React.FC<Props> = () => {
	  return (
	    <>
	      <p>B 組件</p>
	      <C />
	    </>
	  )
	}
	
	const Context = () => {
	  return (
	    <>
	      <BtnProvider>
	        <div>Context</div>
	        <B />
	      </BtnProvider>
	      <D />
	    </>
	  )
	}
	```

	在 `Context` 元件中，被 `BtnProvider` 包裹的 `D` 元件，它的 `btnVisible` 狀態會取得 `true`，沒有被 `BtnProvider` 包裹的 `D` 元件，它的 `btnVisible` 狀態會取得 `false`。