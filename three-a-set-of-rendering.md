
>渲染数组三个分为一组，去掉不满足条件的，一行显示不下字体变小，变小还是显示不下，再显示两行

#three-a-set-of-rendering
*转载需加来源*

*mathName为匹配参数*

```
    arrayToHtml (value) {
        const self = this
        let str = ''
        let num = 0
        let line = ''
        let lengthStr
        value.forEach(function (item) {
            if (self.mathName(item.name)) {
                lengthStr = item.value.length
                if (lengthStr >= 10 && lengthStr < 14) {
                    line = 'line1'
                } else if (lengthStr >= 14) {
                    line = 'line2'
                } else {
                    line = ''
                }
                if (num % 3 === 0) {
                    str += '<div class="itemLis"><li class=' + line + '><div class="left"><i class="c-' + item.name + '"></i><p>' + self.mathName(item.name) + '</p></div><div class="rightCn"><p class="desc">' + item.value + '</p></div></li>'
                } else if (num % 3 === 2) {
                    str += '<li class=' + line + '><div class="left"><i class="c-' + item.name + '"></i><p>' + self.mathName(item.name) + '</p></div><div class="rightCn"><p class="desc">' + item.value + '</p></div></li></div>'
                } else {
                    str += '<li class=' + line + '><div class="left"><i class="c-' + item.name + '"></i><p>' + self.mathName(item.name) + '</p></div><div class="rightCn"><p class="desc">' + item.value + '</p></div></li>'
                }
                num++
            }
        })
        return str
    }
    
    // 调用
    <ul className='cinemaSeriveUl' dangerouslySetInnerHTML={{__html: self.arrayToHtml(serverList)}}></ul>
```

*sass-style*

```
    .cinemaSeriveUl {
        	padding-top:.5rem;
            overflow-x: auto;
            overflow-y: hidden;
            -webkit-overflow-scrolling: touch;
            white-space: nowrap;
        }
        .itemLis {
            width: 12.5rem;
            display: inline-block;
            height: 8.5rem;
            overflow: hidden;
            margin-right: .5rem;
        }
        li {
        	white-space: normal;
        	overflow: hidden;
            margin-bottom: .25rem;
            border-left-width: .25rem;
            border-left-style: solid;
            &:nth-child(6n+0) {
                border-left-color: #ff6600;
            }
            &:nth-child(6n+1) {
                border-left-color: #f0c50b;
            }
            &:nth-child(6n+2) {
                border-left-color: #5dcd35;
            }
            &:nth-child(6n+3) {
                border-left-color: #6e76cb;
            }
            &:nth-child(6n+4) {
                border-left-color: #00c9cc;
            }
            &:nth-child(6n+5) {
                border-left-color: #c26cca;
            }
            width: 12.5rem;
            height: 2.5rem;
            &.line2{
            	.desc {
            		padding-top: .2rem;
	                line-height: 1rem;
	                font-size: .6rem;
	            }
            }
            &.line1{
            	.desc {
	                font-size: .6rem;
	            }
            }
            .desc {
                line-height: 2.5rem;
            }
            .rightCn {
                background-color: $white;
                padding: 0 .5rem;
                margin-left: 3rem;
                height: 2.5rem;
            }
            .left {
                height: 2.5rem;
                padding-top: .25rem;
                background-color: $white;
                text-align: center;
                width: 2.5rem;
                p {
                    font-size: .6rem;
                    color: $gewara-theme-color;
                }
            }
        }
```

