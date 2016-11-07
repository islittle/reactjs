
>React 支持一种非常特殊的属性，你可以用来绑定到 render() 输出的任何组件上去。这个特殊的属性允许你引用 render() 返回的相应的支撑实例（ backing instance ）。
这样就可以确保在任何时间总是拿到正确的实例。

#ref
*转载需加来源*

```
//demo
<div ref='refsname'></div>
// 几种获取方式
this.refs.refsname.getDOMNode();  
React.findDOMNode(this.refs.refsname);  
this.refs['refsname'].getDOMNode();  
React.findDOMNode(this.refs['refsname']); 
```
