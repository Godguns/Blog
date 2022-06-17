##### 首先让我们看一段代码   
```js    
import React, { Component } from "react";

class TextInput extends Component {
  state = {
    editionCounter: 0,
    value: this.props.defaultValue,
  }
  // 由于 setState 是异步操作，event.target.value 在运行时可能已经被重置了
  handleChange = event => 
    this.setState(prevState => ({ value: event.target.value, editionCounter: prevState.editionCounter + 1 }));

  render() {
    return (
      <span>Edited {this.state.editionCounter} times</span>
      <input
        type="text"
        value={this.state.value}
        onChange={this.handleChange} // WRONG!
      />
    )
  }
}
```   
##### 以及这样的代码   
```js   
function onClick(event) {
    setTimeout(() => {
        console.log(event.target.value);
    }， 100);
}
```   
### 这个时候我们要注意 
我们收到的 event 对象为 React 合成事件， event 对象在事件之外不可以使用   为什么会出现这种情况呢？这里我们需要明白一个东西：   
### React在原生的DOM事件上的一层封装，称为SyntheticEvent（合成事件）  ，它有两个特性 ：
1.抹平了不同浏览器和设备之间的事件机制的差异
2，事件池机制
> 下面简单介绍一下事件池机制   
>如果你听说过线程池、Java字符串常量池等“池”的概念，你应该就能秒懂事件池的意思（编程理念很多数都是共通的）

事件池可以形象地理解为有个池子里装满了SyntheticEvent对象，程序有需要时会从池中取出一些使用，使用完后再放回池中。

事件池机制意味着 SyntheticEvent对象会被缓存且反复使用，目的是提高性能，减少创建不必要的对象。当SyntheticEvent对象被收回到事件池中时，属性会被抹除、重置为null。   
##### 因此 我们在写React事件回调函数的时候切记不能将 event 用于异步操作 —— 当异步操作真正执行的时候，SyntheticEvent对象有可能已经被重置了   
## 总结 React SyntheticEvent 封装并兼容了浏览器DOM事件，采用了事件池的机制提升性能同时带来了不能异步使用的弊端     

参考大神链接：[react事件机制](https://blog.51cto.com/u_15064417/2569762)


