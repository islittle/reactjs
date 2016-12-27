
>初始化


#constructor-super

api原文：
>The constructor for a React component is called before it is mounted. When implementing the constructor for a React.Component subclass, you should call super(props) before any other statement. Otherwise, this.props will be undefined in the constructor, which can lead to bugs.

>The constructor is the right place to initialize state. If you don't initialize state and you don't bind methods, you don't need to implement a constructor for your React component.

>It's okay to initialize state based on props if you know what you're doing. 

翻译后:
在mounted之前调用React组件的构造函数。 当实现React.Component子类的构造函数时，应该在任何其他语句之前调用super（props）。 否则，this.props将在构造函数中报未定义错误。

构造函数是要正确方式初始化状态。 如果不初始化状态，并且不绑定方法，则不需要为React组件实现构造函数

如果你知道你在做什么，那么可以依据props来初始化状态。

###举例：

```
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}  
```

