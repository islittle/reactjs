
>这个属性我开始很少用到，还是在一次查看别人的东西的时候忽然看到的。那么接下来我们见到了解一下用途，api稳定说：React.Children提供了处理this.props.children数据结构的实用程序。

#React.Children
/*转载需加来源*/
###React.Children.map
```
React.Children.map(object children, function fn [, object thisArg])
```
example：
子级NotesChildList：
```
render () {
  return (
    <ul>
    {
      React.Children.map(this.props.children, function (child) {
        return <li>{child}</li>
      })
    }
    </ul>
  )
}
```
父级：
```
render (){
   <div>
      <NotesChildList>
        <span>hello</span>
        <span>world</span>
        <span>!</span>
      </NotesChildList>
    </div>
}
```
输出结果为：
hello
world
！
**如果 children 是一个内嵌的对象或者数组，它将被遍历：不会传入容器对象到 fn 中；如果 children 参数是 null 或者 undefined，那么返回 null 或者 undefined 而不是一个空对象**


###React.Children.forEach
```
React.Children.forEach(object children, function fn [, object thisArg])
```
和map不同的是，不返回数组


###React.Children.count
```
React.Children.count(object children)
```
返回 children 中的组件总数，和传递给 map 或者 forEach 的返回函数的调用次数相等


###React.Children.toArray
```
React.Children.only(object children)
```
返回仅有的子对象，否则报错


###React.Children.toArray
```
React.Children.toArray(object children)
```
返回一个由各子元素组成的数组。 如果你想在render方法中操作子元素，特别是如果你想在传递它之前重新排序或切割this.props.children，这将非常有用。**在展开子级列表时React.Children.toArray（）更改键去保留嵌套数组的语义。 也就是说，toArray为返回数组中的每个键赋予前缀，以便将每个元素的键范围限定为包含它的输入数组**


