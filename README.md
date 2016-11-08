# reactjs

>blog-of-reactjs

*转载需加来源*

###周期函数基本执行顺序：

1.**first render：** getDefaultProps->getInitialState->componentWillMount->render->componentDidMount;<br>
2.**unmount：** componentWillUnmount;<br>
3.**secound render：** getInitialState->componentWillMount->render->componentDidMount，但并不执行 getDefaultProps;<br>
4.**props change：**componentWillReceiveProps->shouldComponentUpdate->componentWillUpdate->render->componentDidUpdate;<br>
5.**state change：**shouldComponentUpdate->componentWillUpdate->render->componentDidUpdate;<br>

###三种状态进行管理

1. MOUNTING：mountComponent -> MOUNTING
mountComponent 负责管理生命周期中的 getInitialState、componentWillMount、render 和 componentDidMount

2. RECEIVE_PROPS：updateComponent -> RECEIVE_PROPS
updateComponent 负责管理生命周期中的 componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、render 和 componentDidUpdate
*不建议在 getDefaultProps、getInitialState、render 和 componentWillUnmount 中调用 setState，禁止在 shouldComponentUpdate 和 componentWillUpdate 中调用 setState，会造成循环调用，直至耗光浏览器内存后崩溃，只有在 render 和 componentDidUpdate 中才能获取到更新后的 this.state，*

3. UNMOUNTING：unmountComponent -> UNMOUNTING
unmountComponent 负责管理生命周期中的 componentWillUnmount
*在 componentWillUnmount 中调用 setState，是不会触发 reRender。更新状态为 NULL，完成组件卸载操作。*
