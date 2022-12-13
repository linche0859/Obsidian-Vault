[[React]]

> 可以參考 [Concepts and Data Flow](https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow#redux-application-data-flow) 的附圖，了解資料的運作流程。

# 認識 Redux
## Action 是什麼？
提供執行的動作和資料。

## Dispatch 是什麼？
將 Action 的資料派發給 Reducer。

## Reducer 是什麼？
接收 Action 的資料後，執行邏輯流程，再更新 State。

## Store 是什麼？
單一狀態的儲存庫 (也可以視為 Reducer 的集合)，當有狀態被更新時，會將 State 傳遞給 UI 畫面，讓 UI 畫面重新渲染。

# 基礎設定
1. 定義 Store。
	```ts
	// store.ts
	import { configureStore } from '@reduxjs/toolkit'
	import todoReducer from '@/slices/todoSlice'
	
	export const store = configureStore({
	  reducer: {
	    todo: todoReducer
	  }
	})
	
	// 定義 Store 中的各個 State，這邊會回傳 {todo: TodoState}
	export type RootState = ReturnType<typeof store.getState>
	// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
	export type AppDispatch = typeof store.dispatch
	```

2. 定義 Store 的 Reducer。
	```ts
	// slices/todo.ts
	import { createSlice } from '@reduxjs/toolkit'
	import type { PayloadAction } from '@reduxjs/toolkit'
	
	export interface TodoState {
	  todolist: Array<string>
	}
	
	const initialState: TodoState = {
	  todolist: ['todo one']
	}
	
	export const todoSlice = createSlice({
	  name: 'todo',
	  initialState,
	  reducers: {
	    addTodo: (state, action: PayloadAction<string>) => {
	      state.todolist.push(action.payload)
	    },
	    addTimestamp: (state) => {
	      state.todolist.push(Date.now().toString())
	    }
	  }
	})
	
	// Action creators are generated for each case reducer function
	// 需要給 dispatch 派發的 action
	export const { addTodo, addTimestamp } = todoSlice.actions
	
	export default todoSlice.reducer
	```

3. 主程式的進入點加入 Redux 的 Provider，並將定義好的 store 傳入。
	```tsx
	// index.tsx
	import { Provider } from 'react-redux'
	import { store } from '@/app/store'
	
	const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement)
	root.render(
	  <React.StrictMode>
	    <Provider store={store}>
	      <APP />
	    </Provider>
	  </React.StrictMode>
	)
	```

4. 實作的元件可以：
	- 透過 `useSelector` Hook 取得 Store 的 Reducer State。
		```tsx
		import { useSelector, useDispatch } from 'react-redux'
		import type { RootState } from '@/app/store'
		
		const todoList = useSelector((state: RootState) => state.todo.todolist)
		```

	- 透過 `useDispatch` Hook 指派特定的 Action 給 Reducer，讓 Reducer 更新 State，再重新渲染畫面。
		```tsx
		import { useDispatch } from 'react-redux'
		import { addTodo, addTimestamp } from '@/slices/todoSlice'
		
		const dispatch = useDispatch()
		const [text, setText] = useState('')
		
		dispatch(addTodo(text)) // action 為 {type: 'todo/addTodo', payload: ''}
		dispatch(addTimestamp()) // action 為 {type: 'todo/addTimestamp'}
		```

# 在 Reducer 使用非同步方法
在 Slice 的 `reducers` 屬性內的方法 **沒有辦法使用非同步的方法來設定 Slice 的 State**，所以需要將非同步的函數分開撰寫，再讓元件去呼叫這個非同步的函數。
1. 假如有一個 `userSlice` 是用於紀錄使用者的 State，`userSlice` 可以這麼撰寫。
	```ts
	// store/user/userSlice.ts
	export const userSlice = createSlice({
	  name: 'user',
	  initialState,
	  reducers: {
	    setUsers: (state, action: PayloadAction<User[]>) => {
	      state.users = action.payload
	    }
	  }
	})
	```

2. 撰寫取得使用者列表的函數。
	```ts
	// store/user/userSlice.ts
	import { Dispatch, AnyAction } from 'redux'

	export const fetchUsers = async (dispatch: Dispatch<AnyAction>) => {
	  try {
	    const response = await fetch('https://jsonplaceholder.typicode.com/users')
	    if (!response.ok) {
	      throw new Error('Fetch Fail')
	    }
	    const data = (await response.json()) as User[]
	    dispatch(setUsers(data))
	  } catch (e) {
	    console.log(e)
	  }
	}
	```

	`fetchUsers` 需傳入 `useDispatch` Hook 初始化後的函數。

3. 註冊 `userSlice` 於 store 的 `reducer` 中。
4. 元件內呼叫 `userSlice` 的 `fetchUsers` 函數，並使用 `useSelector` Hook 取得 `userSlice` 的 State 資料。
	```tsx
	import React, { useEffect } from 'react'
	import { useSelector, useDispatch } from 'react-redux'
	import type { RootState } from '../store/index'
	import { fetchUsers } from '../store/user/userSlice'

	const User = () => {
	  const dispatch = useDispatch()
	  const users = useSelector((state: RootState) => state.user.users)
	
	  useEffect(() => {
		  fetchUsers(dispatch)
	  }, [])
	
	  return (
	    <div>
	      <h1>User</h1>
	      <ul>
	        {users?.map((item) => (
	          <li key={item.id}>
	            <div>{item.name}</div>
	            <div>{item.phone}</div>
	            <div>{item.email}</div>
	          </li>
	        ))}
	      </ul>
	    </div>
	  )
	}
	```

雖然可以用上方的方法來實作非同步的執行和設定 Slice 的 State，但 **推薦使用 Redux Toolkit 為較好的管理**，可以參考 [[8.2 RTK Query]] 的內容。

