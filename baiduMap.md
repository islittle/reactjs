
>框架中常用第三方软件，一般都是在加载完成之后执行，有的是通过创建标签插入的方式，然onload后再new，有写head中，直接在onload后new。

#百度地图
*转载需加来源*

**因为在react中componentWillMount是创建demo之前调用，componentDidMount是真实demo创建之后执行。（本人目前没有找到除了在head中引用之外的办法，在
周期函数中创建，然后引用会报错，谁有好的办法可以提commit，多谢）**

```
// html 直接引用
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=秘钥"></script>

// jsx

import styles from './baidumap'
import React, {PropTypes} from 'react'
import {Header} from 'cmpnts'

export default class BaiduMap extends React.Component {

    static propTypes = {
        location: PropTypes.object.isRequired
    }

    componentDidMount () {
        const {location} = this.props
        const {bpointx, bpointy} = location.query
        const heightInt = window.screen.height
        this.refs.myNewMap.style.height = (heightInt - 50) / 20 + 'rem'
        if (bpointx && bpointy) {
            let map = new window.BMap.Map('allmap')
            map.centerAndZoom(new window.BMap.Point(bpointy, bpointx), 15)
            map.clearOverlays()
            let newPoint = new window.BMap.Point(bpointy, bpointx)
            let marker = new window.BMap.Marker(newPoint)  // 创建标注
            map.addOverlay(marker)              // 将标注添加到地图中
            map.panTo(newPoint)
        }
    }

    render () {
        const self = this
        const {location} = self.props
        const cname = '地图 - ' + location.query.cname
        return (
            <div className='BaiduMap'>
                <Header left='back' right='more' center={cname} history={self.props.history} />
                <div ref='myNewMap' id='allmap'></div>
            </div>
        )
    }
}

```
