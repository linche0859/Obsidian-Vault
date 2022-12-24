[[React]]

> 官方文件 ([Tutorial v6.4.3](https://reactrouter.com/en/main/start/tutorial))。

# 路由建置
> 在 v6.4.3 版本上推薦使用 `createBrowserRouter` 的方式建置路由喔。

## 使用 useRoutes Hook
1. 新增 `routes.tsx` 檔案來規劃專案的路由規則。
	```tsx
	// routers.tsx
	import { RouteObject } from 'react-router-dom'
	import Home from '@/pages/home'
	import NotFound from '@/pages/NotFound'
	
	const routes: RouteObject[] = [
	  {
	    path: '/',
	    element: <Home />
	  },
	  {
	    path: '*',
	    element: <NotFound />
	  }
	]
	
	export default routes
	```

2. 在專案的進入點新增路由的模式。
	```tsx
	// index.tsx
	import { BrowserRouter } from 'react-router-dom'

	root.render(
	  <React.StrictMode>
	    <BrowserRouter>
	      <App />
	    </BrowserRouter>
	  </React.StrictMode>
	)
	```

3. 在主要的元件使用 `useRoutes` Hook 繪製路由樹。
	```tsx
	// App.tsx
	import { useRoutes } from 'react-router-dom'
	import routes from '@/routes'
	
	const App: React.FC = () => {
	  const element = useRoutes(routes)
	  return element
	}
	```

## 使用 Routes 和 Route 元件
> 參考官方的 [Routes](https://reactrouter.com/en/main/components/routes)元件說明。

1. 新增路由設定的元件
	```tsx
	// AnimatedRoutes.tsx
	import { Routes, Route, useLocation } from 'react-router-dom'
	import Home from './Home'
	import Gallery from './Gallery'
	import Contact from './Contact'
	
	const AnimatedRoutes: React.FC = () => {
	  const location = useLocation()
	  return (
	    <Routes location={location} key={location.pathname}>
	      <Route path="/" element={<Home />} />
	      <Route path="/gallery" element={<Gallery />} />
	      <Route path="/contact" element={<Contact />} />
	    </Routes>
	  )
	}
	```

	使用 `useLocation` Hook 的原因是，判斷目前正在訪問的路徑，並將路由資訊傳遞給內層的元件 (如：`NavLink` 等) 使用。
	
2. 在主要的元件引入路由設定的元件，和使用 `BrowserRouter` 做包裹。
	```tsx
	// App.tsx
	import { BrowserRouter } from 'react-router-dom'
	import AnimatedRoutes from './routes/framer-motion/AnimatedRoutes'
	import Header from './routes/framer-motion/Header'
	
	const App: React.FC = () => {
	  return (
	    <div>
	      <BrowserRouter>
	        <Header />
	        <AnimatedRoutes />
	      </BrowserRouter>
	    </div>
	  )
	}
	```

3. 自訂導覽列的元件。
	> 可以參考 [NavLink](https://reactrouter.com/en/main/components/nav-link)元件說明。

	```tsx
	// Header.tsx
	import { NavLink } from 'react-router-dom'
	
	const Header = () => {
	  return (
	    <>
	      <nav>
			  <NavLink
				className={({ isActive }) => (isActive ? 'active' : undefined)}
				to="/"
>		
				Home
			  </NavLink>
	
			  <NavLink
				className={({ isActive }) => (isActive ? 'active' : undefined)}
				to="/gallery"
>		
				Gallery
			  </NavLink>
			  <NavLink
				className={({ isActive }) => (isActive ? 'active' : undefined)}
				to="/contact"
>		
				Contact
			  </NavLink>
	      </nav>
	    </>
	  )
	}
	```

# 在 Link 元件上傳遞 state
`Link` 元件上可以加入 `state` 屬性，並將資料傳遞給欲前往的路由元件。
```tsx
const ProductLayout = () => {
  return (
    <>
      <nav>
        <span>
          <Link to="/products/1" state={{ state: 'Product 1' }}>
            Product 1
          </Link>
        </span>
      </nav>
    </>
  )
}
```

匹配到的路由元件可以藉由 `useLocation` Hook 取得 `location` 上的 `state` 值。
```tsx
import { useLocation } from 'react-router-dom'

const Product = () => {
  const location = useLocation()

  console.log(location.state)

  return <div>Product {id}</div>
}
```