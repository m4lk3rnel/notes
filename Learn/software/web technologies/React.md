###### React Hooks 
-  `useState`
-  `useEffect`
-  `useCallback` and `useMemo` accept functions the only difference is `useCallback` returns the function and `useMemo` returns the value of the function
 -  functions in react are different each render
 -  `memo` lets you skip re-rendering a component when its props are unchanged.
 -  If you have a function in a component, even if you return `memo(Component)` the component will be re-render. why? because of the second `functions in react are different each render`. The `useCallback` can be used to `memoize` the function (the component will re-render only when the dependency of `useCallback` will change).
	 -  https://www.youtube.com/watch?v=MxIPQZ64x0I

	 ![[Referential_equality.png]]

-  React does `referential equality` each render. 
-  function === function -> false
	![[useCallback-chatgpt.png]]
-  the `runtime` option in `@babel/preset-react` is used for transforming JSX without using `import React from 'react'` (**automatic JSX runtime**) 