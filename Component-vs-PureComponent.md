
React版本 15.3.0 新增了一个 PureComponent(名字：ReactPureComponent) 类，取代了之前的 react-addons-pure-render-mixin，用法很简单，和React.Compontent很类似，但是PureComponent改变了生命周期方法 shouldComponentUpdate ，它会自动检查组件是否需要重新渲染，只有PureComponent检测到 state 或者 props 发生变化时，PureComponent才会调用 render 方法，达到提升性能的目的。

>React.PureComponent的shouldComponentUpdate方法只浅地比较对象。 如果包含复杂的数据结构，它可能产生更大差异的错误认识和反面特征。 只有当有简单的props和state时，才继承PureComponent，或当你知道深层数据结构已更改时，还是使用forceUpdate（）。 或者考虑使用不可变对象来促进嵌套数据的快速比较。
此外，React.PureComponent的shouldComponentUpdate（）跳过整个组件子树的props更新。 确保所有的子组件也是“纯粹的”。


##PureComponent
*转载需加来源*

```
//用法
import React, { PureComponent } from 'react'

class Example extends PureComponent {
  render() {
    // ...
  }
}
或者
import React from 'react'

class Example extends React.PureComponent {
  render() {
    // ...
  }
}
```
