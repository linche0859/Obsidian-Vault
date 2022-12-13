[[React]]

使用 `styled-components` 套件封裝每個元件的樣式，讓元件的樣式不會影響到其他元件。
>`styled-components` 的[官方文件](https://styled-components.com/docs)。

# 使用步驟
1. 安裝 `styled-components` 套件。
	```bash
	yarn add styled-components @types/styled-components -D
	```

2. 撰寫 styled 元件。
	```tsx
	const H1 = styled.h1`
	  color: #00f;
	`
	```

3. 引用於預設的 `export` 元件。
	```tsx
	const H1 = styled.h1`
	  color: #00f;
	`
	
	type BasicButtonType = {
	  primary: boolean
	}
	const BasicButton = styled.button<BasicButtonType>`
	  background: ${(props) => (props.primary ? 'palevioletred' : 'white')};
	  color: ${(props) => (props.primary ? 'white' : 'palevioletred')};
	  font-size: 1em;
	  margin: 1em;
	  padding: 0.25em 1em;
	  border: 2px solid palevioletred;
	  border-radius: 3px;
	`
	
	const StyledComponent = () => {
	  const [primary, setPrimary] = useState(false)
	  return (
	    <>
	      <H1>Hello World</H1>
	      <BasicButton primary={primary}>Primary</BasicButton>
	      <button onClick={() => setPrimary(!primary)}>change primary</button>
	    </>
	  )
	}
	```

	- `${ props => ... }` -> 在 styled 元件中使用 props 的內容來控制樣式。
	- `styled.HTML 標籤<泛型>` -> 定義 styled 元件的 props 內容。
