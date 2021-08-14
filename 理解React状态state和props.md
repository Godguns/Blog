### 定义State   
State作为组件的私有属性，主要用于对组件的私有属性进行管理，通过对属性的状态的监听去渲染UI，从而完成用户数据和界面展示的一致性。

### State 与 Props   
除了State, 组件的Props也是和组件的UI展示有关的。他们之间的主要区别是：State是可变的，是组件内部维护的一组用于反映组件UI变化的状态集合；而Props对于使用它的组件来说，是只读的，要想修改Props，只能通过该组件的父组件修改。在组件状态上移的场景中，父组件正是通过子组件的Props, 传递给子组件其所需要的状态。   
### 修改State的正确姿势   

```
1.不能直接修改State。
在React中，直接修改state并不会触发render函数，所以下面的写法是错误的。

// 错误
this.state.title = 'React';
组件的State只能通过setState()方式进行修改。例如：

// 正确
this.setState({title: 'React'});
```   
### State 的更新是异步的
调用setState，组件的state并不会立即改变，setState只是把要修改的状态放入一个队列中，React会优化真正的执行时机，并且React会出于性能原因，可能会将多次setState的状态修改合并成一次状态修改。==所以不要依赖当前的State，计算下个State。当真正执行状态修改时，依赖的this.state并不能保证是最新的State，因为React会把多次State的修改合并成一次，这时，this.state将还是这几次State修改前的State==。另外需要注意的事，同样不能依赖当前的Props计算下个状态，因为Props一般也是从父组件的State中获取，依然无法确定在组件状态更新时的值。   
### 举个例子   
> 举个例子，对于一个电商类应用，在我们的购物车中，当我们点击一次购买数量按钮，购买的数量就会加1，如果我们连续点击了两次按钮，就会连续调用两次this.setState({quantity: this.state.quantity + 1})，在React合并多次修改为一次的情况下，相当于等价执行了如下代码：


```
Object.assign(
  previousState,
  {quantity: this.state.quantity + 1},
  {quantity: this.state.quantity + 1}
)
```

于是乎，后面的操作覆盖掉了前面的操作，最终购买的数量只增加了1个。==如果我们要实现加2的效果，可以使用另一个接收一个函数作为参数的setState，这个函数有两个参数，第一个是当前最新状态（本次组件状态修改后的状态）的前一个状态preState（本次组件状态修改前的状态），第二个参数是当前最新的属性props。==

```
// 正确
this.setState((preState, props) => ({
  counter: preState.quantity + 1; 
}))
```   
### State 的更新是一个浅合并的过程   
当调用setState修改组件状态时，只需要传入发生改变的State，而不是组件完整的State，因为组件State的更新是一个浅合并（Shallow Merge）的过程。例如，一个组件的状态为：


```
this.state = {
  title : 'React',
  content : 'React is an wonderful JS library!'
}
```   
当只需要修改状态title时，只需要将修改后的title传给setState即可。   

```
this.setState({title: 'Reactjs'});
```   
React会合并新的title到原来的组件状态中，同时保留原有的状态content，合并后的State的内容为：   

```
{
  title : 'Reactjs',
  content : 'React is an wonderful JS library!'
}

```   
### State与不可变对象   
==React官方建议把State当作是不可变对象，一方面是如果直接修改this.state，组件并不会重新render；另一方面State中包含的所有状态都应该是不可变对象。当State中的某个状态发生变化，我们应该重新创建这个状态对象，而不是直接修改原来的状态。==   
#### 1. 状态的类型是不可变类型（数字，字符串，布尔值，null， undefined）
这种情况最简单，因为状态是不可变类型，直接给要修改的状态赋一个新值即可。例如：这种情况最简单，因为状态是不可变类型，直接给要修改的状态赋一个新值即可。例如：   

```
this.setState({
  count: 1,
  title: 'Redux',
  success: true
})

```   
#### 2.状态的类型是数组   
如有一个数组类型的状态books，当向books中增加一本书时，使用数组的concat方法或ES6的数组扩展语法（spread syntax）即可。   
关于concat在MDN文档里面的定义是：   
> concat() 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。   

```
// 方法一：将state先赋值给另外的变量，然后使用concat创建新数组
var books = this.state.books; 
this.setState({
  books: books.concat(['React Guide']);
})

// 方法二：使用preState、concat创建新数组
this.setState(preState => ({
  books: preState.books.concat(['React Guide']);
}))

// 方法三：ES6 spread syntax
this.setState(preState => ({
  books: [...preState.books, 'React Guide'];
}))
```   
当需要从books中截取部分元素作为新状态时，使用数组的slice方法。例如：   

```
// 方法一：将state先赋值给另外的变量，然后使用slice创建新数组
var books = this.state.books; 
this.setState({
  books: books.slice(1,3);
})

// 方法二：使用preState、slice创建新数组
this.setState(preState => ({
  books: preState.books.slice(1,3);
}))
```   
==注意：不要使用push、pop、shift、unshift、splice等方法修改数组类型的状态，因为这些方法都是在原数组的基础上修改，而concat、slice、filter会返回一个新的数组。==   
#### 3. 状态的类型是普通对象（不包含字符串、数组）   
1，使用ES6 的Object.assgin方法。   

```
// 方法一：将state先赋值给另外的变量，然后使用Object.assign创建新对象
var owner = this.state.owner;
this.setState({
  owner: Object.assign({}, owner, {name: 'Jason'});
})

// 方法二：使用preState、Object.assign创建新对象
this.setState(preState => ({
  owner: Object.assign({}, preState.owner, {name: 'Jason'});
}))
```   
使用对象扩展语法   

```
// 方法一：将state先赋值给另外的变量，然后使用对象扩展语法创建新对象
var owner = this.state.owner;
this.setState({
  owner: {...owner, name: 'Jason'};
})

// 方法二：使用preState、对象扩展语法创建新对象
this.setState(preState => ({
  owner: {...preState.owner, name: 'Jason'};
}))
```











