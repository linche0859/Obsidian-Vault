[[React]]

以 [[2 Props State]] 中 Props 段落的範例改寫成 Class Component 的方式：
```tsx
type AddBtnProps = {
  count: number
  onClickHandler: (value: number) => void
}
type AddBtnState = {}
class AddBtn extends React.Component<AddBtnProps, AddBtnState> {
  private addValue = 2
  componentWillUnmount() {
    console.log('AddBtnState Unmount')
  }
  render(): React.ReactNode {
    return (
      <button onClick={() => this.props.onClickHandler(this.addValue)}>
        加 {this.addValue}，Btn 的計數器：{this.props.count}
      </button>
    )
  }
}

type CounterProps = {}
type CounterState = {
  count: number
}
class Counter extends React.Component<CounterProps, CounterState> {
  constructor(props: CounterProps) {
    super(props)
    this.state = {
      count: 0
    }
    this.clickHandler = this.clickHandler.bind(this)
  }
  clickHandler(value: number) {
    this.setState((prevState) => ({ count: prevState.count + value }))
  }
  componentDidMount() {
    console.log('mount')
  }
  componentDidUpdate() {
    console.log('update', this.state)
  }
  render(): React.ReactNode {
    return (
      <>
        <p>Counter 的計數器：{this.state.count}</p>
        {this.state.count < 5 ? (
          <AddBtn count={this.state.count} onClickHandler={this.clickHandler} />
        ) : null}
      </>
    )
  }
}
```

補充的要點有以下：
- 使用 `state` 紀錄資料狀態和畫面的綁定關係，並在 `constructor` 函數中做初始值的設定。
- 元件中的方法需要呼叫 `bind` 函數綁定元件的 `this` 狀態，否則透過 props 傳入子元件時，會發生 `this` 指向 `undefinded` 的錯誤。
- Class Component 的生命週期有
	- `componentDidMount` -> 元件掛載於畫面上時觸發。
	- `componentDidUpdate` -> 元件內 state 的狀態變更時觸發。
	- `componentWillUnmount` -> 元件將被移除時觸發。