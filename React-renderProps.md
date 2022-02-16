React 中组件复用主要有两种模式，一种是高阶组件另一种就是 render props 模式

这两种方式并不是新的 API ，而是利用 React 自身特点的编码技巧，演化而来的固定模式

render props 模式是一种非常灵活并且复用性非常高的模式，可以将一些特定的行为或功能封装成一个组件供别的组件使用。

主要实现思路：

将要复用的 state 状态和操作 state 的方法封装到一个组件中

使用 复用组件 时为其传递一个函数，通过这个函数来获取 复用组件 中的 state （将 state 暴露到 复用组件的外部）

这个函数的返回值为需要渲染的 UI 内容（渲染的 DOM 结构由传入的函数决定）

举个例子：比如将获取鼠标的坐标、鼠标移动事件封装成一个可复用组件.  


当使用的组件有子节点时，其 props 中会多一个 children 属性，表示组件标签的子节点；children 属性与普通的 props 一样，值可以是任意值（文本、react元素、组件、甚至是函数）   
```   

import React from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

import img from './images/tianshi.gif'

// 根组件
class App extends React.Component {
  render () {
    return (
      <div>
        <h1>App 组件</h1>        
        {/* 改造为 children 模式，将函数作为子节点 */}
        {/* 坐标信息 */}
        <Mouse>
          {
            mouse => (
              <div>
                <p>X坐标：{mouse.x}，Y坐标：{mouse.y}</p>
              </div>
            )
          }
        </Mouse>

        {/* 图片 */}
        <Mouse>
          {
            mouse => (
              <img
                src={img}
                alt="cat"
                style={{
                  position: 'absolute',
                  left: mouse.x - 64,
                  top: mouse.y - 64
                }} />
            )
          }
        </Mouse>
      </div>
    )
  }
}

// 创建复用组件
class Mouse extends React.Component {
  // 校验；render 必须传入且值为函数
  static propTypes = {
    children: PropTypes.func.isRequired
  }
  // 1. 状态
  state = {
    x: 0,
    y: 0
  }

  render () {
    // DOM 结构由使用者来决定
    // 3. 调用外部传递的函数，并将组件状态作为参数传递给这个函数
    return this.props.children(this.state)
  }

  // 2.1 获取鼠标坐标的函数，每调用一次就修改一次状态
  handleMouseMove = (e) => {
    this.setState({
      x: e.clientX,
      y: e.clientY
    })
  }
  // 2.2 在挂载之后注册鼠标移动事件
  componentDidMount () {
    window.addEventListener('mousemove', this.handleMouseMove)
  }
  // 组件卸载时解绑鼠标移动事件
  componentWillUnmount () {
    // 清除绑定的事件
    window.removeEventListener('mousemove', this.handleMouseMove)
  }
}
ReactDOM.render(<App />, document.getElementById('root'))
```
