
>提交代码后，忽然发现我手机自带得浏览器页面不显示(本人华为荣耀8，浏览器基于腾讯X5内核)，然后我又试了一下，手机上得QQ，微信都是这样的问题。而我手机上
chrome浏览器可以正常显示。然后知道愿意就在pc上用微信开发者工具，或者QQ电脑版去追究问题，但是pc上确显示正常。然后我就走上了debug之路，经过不断的debug，
修后发现for of在X5中居然不执行，然后我换成了foreach，for(,,),for in后确发现神奇的好了，就此文章，提醒还在寻求之路的developer

#for-of-in-moblie-tencent-x5-die

```
// react 中本人用了map
arraylist.map(function(item,key){
  console.log(item,key)
})
```
