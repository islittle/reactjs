
>图片上传经常用得一个功能，React-Dropzone-Component，一个比较好用的插件，可以drop或者click，进度条，添加，删除等等满足你需要的功能。

#use-react-dropzone-component
*转载需加来源*

###安装 react-dropzone-component
npm install react-dropzone-component --save

###配置使用
> 我这里主要用到的就是上传进度条显示，filename，success-mask，error-mark，size都要隐藏掉，添加窗，删除图标按钮，以及传到父级调用
####Uploaded.js
`
import styles from './Upload'
import { Component, PropTypes } from 'react'
import DropzoneComponent from 'react-dropzone-component'

export default class Upload extends Component {
    static propTypes = {
        WalaWriteUpList: PropTypes.func
    }

    constructor (props) {
        super(props)
        const self = this
        // 参数设置
        self.djsConfig = {
            addRemoveLinks: true,
            acceptedFiles: 'image/jpeg,image/png,image/gif'
        }

        self.componentConfig = {
            iconFiletypes: ['.jpg', '.png', '.gif'],
            showFiletypeIcon: true,
            postUrl: `${API}/wapi/touch/wala/uploadPicture.xhtml`
        }

        self.callbackArray = [
            function () {
                console.log('I\'m a callback in an array!')
            },
            function () {
                console.log('Wooooow,callback!')
            }
        ]

        self.callback = () => (function () {
            console.log('upload success')
        })()

        self.success = files => (function () {
            let tempValue = JSON.parse(files['xhr']['response']).data.picPath
            self.props.WalaWriteUpList({'value': tempValue, 'style': 'add'}) 
            // 这里回调ajax异步成功拿到图片途径，下面删除同样
        })()

        self.removedfile = files => (function () {
            let tempValue = JSON.parse(files['xhr']['response']).data.picPath
            self.props.WalaWriteUpList({'value': tempValue, 'style': 'remove'})
        })()

        self.dropzone = null
    }

    render () {
        const config = this.componentConfig
        const djsConfig = this.djsConfig

        // 回调函数
        const eventHandlers = {
            init: dz => this.dropzone = dz,
            drop: this.callbackArray,
            addedfile: this.callback,
            success: this.success,
            removedfile: this.removedfile
        }

        return (
            <div className='UploadImg clearfix'>
                <DropzoneComponent config={config} eventHandlers={eventHandlers} djsConfig={djsConfig} />
            </div>
        )
    }
}

`


####Upload.scss：

`
@import 'scss/const';
.WalaDetail {
    font-size: .7rem;
    line-height: 1.2rem;
    color: $grey-dark;
    padding-bottom: 3rem;
    .topBigPic {
        text-align: center;
        min-height: 6.5rem;
        background-color: $grey-dd;
        img {
            max-width: 100%;
        }
    }
    .contentInfo {
        margin-top: -1rem;
        background-color: $white;
        position: relative;
        border-top-left-radius: 1rem;
        border-top-right-radius: 1rem;
        .userHeader {
            width: 3.5rem;
            height: 3.5rem;
            margin: 0 auto;
            margin-bottom: .2rem;
            margin-top: -1.75rem;
            img {
                width: 100%;
                height: 100%;
                border: 0.05rem solid $white;
                @include border-radius(50%)
            }
        }
        .userName {
            margin-bottom: .3rem;
        }
        .followInfo {
            display: inline-block;
            padding: 0 .8rem;
            @include border-value(.05rem, solid, $gewara-theme-color, .6rem);
            @include textheight(1.2rem, 1.2rem);
            color: $gewara-theme-color;
            margin-bottom: .4rem;
            a {
                color: $gewara-theme-color;
            }
        }
    }
    .contentInfoStart {
        .Stars {
            float: inherit;
        }
        span {
            display: inline-block;
            color: $gewara-theme-color;
            font-size: .65rem;
            height: .9rem;
            line-height: .9rem;
            vertical-align: text-bottom;
            margin-bottom: .15rem;
            padding-left: .2rem;
        }
    }
    .contentInfoCn {
        padding: 0 4%;
        h1.tittle {
            font-size: 1.2rem;
            line-height: 1.2;
            margin-bottom: .75rem;
            border-bottom: $border;
            padding: .25rem 0 .75rem;
        }
        .commentPicName {
            width: 100%;
            margin-bottom: 1rem;
            .imgList {
                width: 100%;
                text-align: center;
                margin-bottom: .25rem;
            }
            .imgList2 {
                width: 100%;
                @include display-flex();
                .imgCn {
                    @include flex(1);
                    margin-right: .25rem;
                    min-height: 4.6rem;
                    overflow: hidden;
                    &:last-child {
                        margin-right: 0
                    }
                }
            }
            .imgList img,
            .imgList2 img {
                width: 100%;
                background-size: cover;
                background-position: center center;
                background-repeat: no-repeat;
                background-color: transparent;
            }
        }
        .cnInfo {
            p {
                font-size: .8rem;
                line-height: 1.3rem;
            }
            img {
                max-width: 100%;
                text-align: center;
                display: inline-block;
                margin-bottom: .2rem;
            }
        }
        .showAddTime {
            color: #999999;
        }
        .codeInfo {
            border-top: $border;
            margin-bottom: 1.5rem;
            img {
                width: 4rem;
                height: 4rem;
                margin: 1.5rem auto;
            }
            color: #7f7f7f;
        }
    }
    .otherInfoCn {
        background: $theme-bgcolor;
    }
    .walaBgIcon {
        display: inline-block;
        background: url(~assets/images/walaNewIco.png) 0 0 no-repeat;
        vertical-align: middle;
    }
    .smartHeart {
        @extend .walaBgIcon;
        background-size: .8rem auto;
        background-position: .25rem -4.5rem;
        width: 1.25rem;
        font-size: .6rem;
        line-height: .6rem;
        color: $gewara-theme-color;
        height: 1.5rem;
        padding-top: .9rem;
        margin-right: .8rem;
        color: $gewara-theme-color;
    }
    .followUserList {
        padding: 1rem 4%;
        border-top: $border;
        border-bottom: $border;
        li {
            img {
                width: 1.5rem;
                height: 1.5rem;
                @include border-radius(.75rem)
            }
            margin-right: .4rem;
        }
        .more {
            font-size: 1.2rem;
            height: 1.5rem;
            line-height: 1rem;
            color: #7f7f7f;
        }
    }
    .themeColor {
        color: $gewara-theme-color;
    }
    .commentList {
        padding: 0 4%;
        line-height: 1.2rem;
        .item-li {
            border-bottom: $border;
            padding: .75rem 0;
            position: relative;
            .ui_pic {
                margin-right: .5rem;
                float: left;
                img {
                    @include border-radius(50%) width: 1.5rem;
                    height: 1.5rem;
                }
            }
            .ui_text {
                font-size: .6rem;
                margin-left: 2rem;
                line-height: .7rem;
                .walaTime {
                    color: #CCC
                }
            }
            .walaText {
                margin-top: .75rem;
            }
            .toZan {
                @extend .walaBgIcon;
                position: absolute;
                height: 1.25rem;
                right: 0;
                top: .75rem;
                padding-left: 1.2rem;
                background-size: 1.25rem auto;
                background-position: 0 -4.4rem;
                line-height: 2rem;
                font-size: .6rem;
                color: #a0a0a0;
                &.select {
                    background-position: 0 -2.05rem;
                    color: $gewara-theme-color;
                }
                .bigNum {
                    font-size: 1.5rem;
                    line-height: 1;
                    font-weight: bold;
                    position: absolute;
                    top: -100%;
                    left: 0;
                }
            }
        }
        .getMoreWala {
            border: $border;
            padding: .5rem 0;
        }
    }
    .commentListTab {
        padding: .75rem 0 .25rem;
        border-bottom: $border;
        font-size: .6rem;
        color: #a0a0a0
    }
    .walaFootMenu {
        position: fixed;
        left: 0;
        bottom: 0;
        width: 100%;
        height: 2.3rem;
        border-top: .05rem solid $grey-c0;
        background: $theme-bgcolor;
        padding: .25rem 4%;
        .toLink {
            display: inline-block;
            background: url(~assets/images/zan.png) 0 0 no-repeat;
            vertical-align: middle;
            background-position: 0 -1.6rem;
            background-size: 100% auto;
            color: $white;
            width: 1.8rem;
            height: 1.8rem;
            line-height: 1.5rem;
            &.select {
                background-position: 0 .05rem;
            }
        }
        .comments {
            background-color: $white;
            @include border-radius(.9rem);
            @include textheight(true, 1.8rem);
            margin-left: 2.3rem;
            input {
                width: 62%;
                margin-left: .5rem;
            }
            a.toPush {
                margin-left: .4rem;
                padding: 0 .4rem;
                border-left: .05rem solid #a0a0a0;
                color: $gewara-theme-color;
            }
        }
        input {
            border: 0 none;
            outline: none;
            resize: none;
            color: $grey-dark;
        }
        .icoEmjo {
            @extend .walaBgIcon;
            background-position: center -28.6rem;
            background-size: 1.4rem auto;
            height: 1.2rem;
            width: 1.2rem;
        }
        .iconCenter {
            position: absolute;
            top: -.05rem;
            left: 0;
            width: 100%;
            background: $white;
            @include translate3D(0, -100%, 0) .iconHead {
                border-top: .05rem solid $grey-c0;
                border-bottom: .05rem solid $grey-c0;
                color: $blue-dark;
                padding: .25rem 4%;
                text-align: right;
            }
            .iconBox {
                padding: 0 4% .5rem;
            }
            li {
                width: 14.2%;
                float: left;
                text-align: center;
                padding-top: .5rem;
            }
            img {
                width: 1.5rem;
                height: 1.5rem;
            }
        }
    }
}

`

####父级调用:(**注意cmpnts是我存放插件的目录**)

`
import {Uploaded} from 'cmpnts'
let upPicture = []
constructor (props) {
        super(props)
        this.state = {
            WalaWriteUpList: []
        }
    }
    
 @bind()
    WalaWriteUpList (value) {
        const self = this
        let valueTemp = value.value
        if (value.style === 'add') {
            upPicture.push(valueTemp)
        } else {
            upPicture = self.removeArray(upPicture, valueTemp)
        }
    } // 这里是操作回调接受函数，最后得到的upPicture就是需要传给接口上传图片路径接口
    
<Uploaded WalaWriteUpList={self.WalaWriteUpList} />
`

更多用法：

[React-Dropzone-Component链接](https://github.com/felixrieseberg/React-Dropzone-Component)

