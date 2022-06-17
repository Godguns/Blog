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
我们收到的 event 对象为 React 合成事件， event 对象在事件之外不可以使用   
