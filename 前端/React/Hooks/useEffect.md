React不能在渲染时产生副作用，所以不能在组件代码中直接修改状态或操作DOM，可以使用`useEffect` hook。Effect会在渲染后产生副作用。

### 1. 依赖数组
#### 触发规则
`useEffect`的触发时机取决于依赖数组，如果没有依赖数组，将在每次渲染后触发
```tsx
useEffect(() => {
  // 这里的代码会在每次渲染后运行
});

useEffect(() => {
  // 这里的代码只会在组件挂载（首次出现）时运行
}, []);

useEffect(() => {
  // 这里的代码不但会在组件挂载时运行，而且当 a 或 b 的值自上次渲染后发生变化后也会运行
}, [a, b]);
```
#### 浅比较陷阱
`useEffect`的依赖数组对比执行的是浅比较，如果参数中是是引用，当引用值没变但地址变化时也会触发Effect
```jsx
function App() {
  // ❌ 错误写法
  // 每次 App 渲染，options 都会被重新创建（内存地址变了）
  const options = { server: 'https://api.com' }; 

  useEffect(() => {
    // 因为 options 地址每次都变，React 认为它变了
    // 导致这个 Effect 每次渲染都跑，导致死循环或性能浪费
    fetchData(options);
  }, [options]); 
}
```

### 2. 清除函数
`useEffect`的返回值是一个清除函数
```tsx
import { useState, useEffect } from 'react'

const Son = () => {
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('Son Component Interval')
    }, 1000);

	// 清除副作用，如果没有清除函数，timer会不断累加
    return () => clearInterval(timer);
  }, [])

  return <div>Son Component</div>;
}

function App() {
  const [show, setShow] = useState(true)

  return (
    <>
      {show && <Son />};
      <button onClick={() => setShow(!show)}>SHOW</button>
    </>
  )
}

export default App
```
清除函数最常用的执行时机是在组件卸载时执行

以下是一个`useEffect`的使用问题
```jsx
/* 有A、B、C三个字段，B依赖于A，C依赖于A和B，如果在useEffect的依赖数组里同时监听A和B，在A变化后B也会发生变化，但是在A变化时就已经触发了C的变化，会导致C变化两次。 */
useEffect(() => {
	setB();
}, [a]);
useEffect(() => {
	setC();
}, [a, b]);
// 所以如果要正确更新C，只能监听字段A，然后在函数体内先计算出更新后的B和C，再更新值
useEffect(() => {
	newB =
	newC =
	setB();
	setC();
}, [a])
```


