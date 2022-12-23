[[React]]

> [React Query 的官方文件](https://react-query-v3.tanstack.com/overview)

React Query 常用來載入 API 的資料，並提供快取、載入的狀態、客製化的載入事件等。

# 動手實作
1. 建立一個元件，接著使用 React Query 提供的 `QueryClientProvider` 包裹元件的內容。
	```tsx
	import React from 'react'
	import { QueryClient, QueryClientProvider } from 'react-query'
	import { ReactQueryDevtools } from 'react-query/devtools'
	import Pokemon from './Pokemon'
	import Pokemons from './Pokemons'
	
	const queryClient = new QueryClient()
	
	const Query = () => {
	  return (
	    <QueryClientProvider client={queryClient}>
	      <Pokemon />
	      <hr />
	      <Pokemons />
	      <ReactQueryDevtools />
	    </QueryClientProvider>
	  )
	}
	```

	引用 `ReactQueryDevtools` 的原因為，React Query 提供下載資料的 DevTool，可以方便查看每一次的請求狀態和結果。

2. 建立 `Pokemon` 元件，這個元件主要用於取得單一筆資料。
	```tsx
	// Pokemon.tsx
	import React, { useState } from 'react'
	import { useQuery } from 'react-query'
	
	const fetchPokemon = async ({ queryKey }: { queryKey: (string | number)[] }) => {
	  const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${queryKey[1]}`)
	  const data = await response.json()
	  return data
	}
	
	const Pokemon: React.FC = () => {
	  const [dexId, setDexId] = useState(1)
	  const { data, isSuccess, isError, isLoading, error, dataUpdatedAt } = useQuery(['PokemonApi', dexId], fetchPokemon, {
	    retry: 0,
	    // cacheTime: 1000,
	    refetchOnWindowFocus: false // 切換網頁標籤時，是否需要重新載入
	  })
	
	  if (isLoading) {
	    return <h1>Loading...</h1>
	  }
	  if (isError) {
	    return <h1>{error instanceof Error ? error.message : 'Something wrong.'}</h1>
	  }
	
	  return (
	    <>
	      <h3>Data Updated At: {new Date(dataUpdatedAt).toLocaleString()}</h3>
	      <p>
	        No.00{data.id} {data.name}
	      </p>
	      <img src={data?.sprites?.front_default} alt={data.name} />
	      <ul>
	        {data?.types?.map((item: { slot: number; type: { name: string } }, index: number) => (
	          <li key={item.slot}>{item.type.name.toUpperCase()}</li>
	        ))}
	      </ul>
	      <button
	        onClick={() => {
	          setDexId((prev) => prev - 1)
	        }}
      >	
	        Previous
	      </button>
	      <button
	        onClick={() => {
	          setDexId((prev) => prev + 1)
	        }}
      >	
	        Next
	      </button>
	    </>
	  )
	}
	```

	在點擊 Previous 或 Next 按鈕時，寫入新的 `dexId` 值，再觸發 `useQuery` Hook 執行其中的 Callback (`fetchPokemon`)。`fetchPokemon` 方法再透過 `useQuery` Hook 的第一個參數，產生一個包含 `queryKey` 屬性的物件，讓 `featch` 方法可以取用。