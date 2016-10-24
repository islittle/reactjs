
> 滚动加载监控 常用

*转载需加来源*

#vertical-scroll
>主要监控滚动条的位置

```
let num = 1
let isLock = true
// 两个全局变量 设置开关

// 下面为export default class extends Component中需要的代码
constructor (props) {
        super(props)
        this.state = {
            isShowGetMore: true,
            isShowGetBtn: '加载更多哇啦'
        }
    }
    @bind() // 为自己封装的construtor函数，防止每次访问创建
    getMoreFun () {
        const self = this
        const {actions, relatedid, tag} = this.props
        const params = {
            pageNo: num,
            relatedid: relatedid,
            tag: tag
        }
        self.setState({isShowGetBtn: '加载中...'})
        loaded.show()
        actions.upDate(false)
        actions.fetchWalaList(params).then(function (value) {
            value.retval.commentVoList.length < 10 && self.setState({isShowGetMore: false})
            self.setState({isShowGetBtn: '加载更多'})
            num++
            isLock = true
            loaded.hide()
        })
    }

    componentDidMount () {
        window.addEventListener('scroll', this.handleScroll)
    }

    componentWillUnmount () {
        window.removeEventListener('scroll', this.handleScroll)
    }

    @bind()
    handleScroll () {
        let self = this
        let obj = document.getElementById('demoUl') // 滚动需改改动的盒子
        let height = window.screen.height
        let length = self.props.WalaListState.commentVoList.length // 获加载的数字
        if (obj && obj.offsetTop + obj.offsetHeight - document.body.scrollTop <= height && self.state.isShowGetMore && length >= 10) {
            if (num === 1) {
                isLock && self.getMoreFun()
            } else if (num !== 1 && isLock) {
                self.getMoreFun()
            }
            isLock = false
        } else {
            return false
        }
    }
```



# horizontal-scroll 
>主要监控最后一个元素的坐标位置

```
    @bind()
    handleScroll () {
        const oDemo = document.getElementsByClassName('cinemaMovieImg')[0].childNodes
        let oLength = oDemo.length - 1
        let lastDemo = oDemo[oLength]
        let leftLeave = lastDemo.getBoundingClientRect().left
        const isWidth = window.screen.width - 72
        const self = this
        if (leftLeave <= isWidth) {
            if (num === 1) {
                isLock && self.getMoreFun()
            } else if (num !== 1 && isLock) {
                self.getMoreFun()
            }
            isLock = false
        } else {
            return false
        }
    }
```

