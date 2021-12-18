## redux 概念图   
![image](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/12/15/16f09a0b5196a2dd~tplv-t2oaga2asx-watermark.awebp)      
### view   
首先我们来看 View ，在前端开发中，我们称这个为视图层，就是展示给最终用户的效果，在本篇教程的学习中，我们的 View 就是 React。   
### store   
随着前端应用要完成的工作越来越丰富，我们对前端也提出了要保持 “状态” 的要求。在 React 中，这个 “状态” 将保存在 this.state。在 Redux 中，这个状态将保存在 Store。
这个 Store 从抽象意义上来说可以看做一个前端的 “数据库”，它保存着前端的状态（state），并且分发这些状态给 View，使得 View 根据这些状态渲染不同的内容。
注意到，Redux 是一个可预测的 JavaScript 应用状态管理容器，这个状态容器就是这里的 Store。   
###  Reducers   
我们日常生活中看到的网页，它不是一成不变的，而是会响应用户的 “动作”，无论是页面跳转还是登陆注册，这些动作会改变当前应用的状态。
在 Redux 框架中，Reducers 的作用就是响应不同的动作。更精确地说，Reducers 是负责更新 Store 中状态的 JavaScript 函数。
