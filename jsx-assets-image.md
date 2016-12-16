
> 页面中引用图片

#jsx-assets-image

*转载需加来源*


`
如assets是资源路径，下面有一个image文件夹存放图片

// 第一种方法

import LogoImg from 'assets/image/logo.png'

<img src={LogoImg}/>


// 第二种方法

<img src={require('assets/image/logo.png')} />

`


