```javascript   
<div id="mount-point"></div>
// 创建构造器
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
结果如下：

<p>Walter White aka Heisenberg</p>   
```   
可以看到，extend 创建的是 Vue 构造器，而不是我们平时常写的组件实例，所以不可以通过 new Vue({ components: testExtend }) 来直接使用，需要通过 new Profile().$mount('#mount-point') 来挂载到指定的元素上。   
### 为什么要使用Vue.extend   
在 vue 项目中，我们有了初始化的根实例后，所有页面基本上都是通过 router 来管理，组件也是通过 import 来进行局部注册，所以组件的创建我们不需要去关注，相比 extend 要更省心一点点。但是这样做会有几个缺点：    

组件模板都是事先定义好的，如果我要从接口动态渲染组件怎么办？   

所有内容都是在 #app 下渲染，注册组件都是在当前位置渲染。如果我要实现一个类似于 window.alert() 提示组件要求像调用 JS 函数一样调用它，该怎么办？
这时候，Vue.extend + vm.$mount 组合就派上用场了。   
### 简单例子   
```javascript   
我们照着官方文档来创建一个示例：

import Vue from 'vue'

const testComponent = Vue.extend({
  template: '<div>{{ text }}</div>',
  data: function () {
    return {
      text: 'extend test'
    }
  }
})
然后我们将它手动渲染：

const extendComponent = new testComponent().$mount()
这时候，我们就将组件渲染挂载到 body 节点上了。

我们可以通过 $el 属性来访问 extendComponent 组件实例：

document.body.appendChild(extendComponent.$el)   
```
