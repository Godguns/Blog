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



