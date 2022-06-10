#### setState.  
除非 shouldComponentUpdate() 返回 false，否则 setState() 将始终执行重新渲染操作。   
如果可变对象被使用，且无法在 shouldComponentUpdate() 中实现条件渲染，那么仅在新旧状态不一时调用 setState()可以避免不必要的重新渲染.    
###### 假设我们想根据 props.step 来增加 state：   
```javascript   
this.setState((state, props) => {
  return {counter: state.counter + props.step};
});   
```   
###### 通常，我们建议使用 componentDidUpdate() 来代替此方式。      
##### 关于setState是同步还是异步的总结    
只要你进入了 react 的调度流程，那就是异步的。只要你没有进入 react 的调度流程，那就是同步的。什么东西不会进入 react 的调度流程？ setTimeout setInterval ，直接在 DOM 上绑定原生事件等。这些都不会走 React 的调度流程，你在这种情况下调用 setState ，那这次 setState 就是同步的。 否则就是异步的。

而 setState 同步执行的情况下， DOM 也会被同步更新，也就意味着如果你多次 setState ，会导致多次更新，这是毫无意义并且浪费性能的。



