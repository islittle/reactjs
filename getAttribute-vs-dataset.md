
> 获取html5自定义属性data-*，获取其值，我们经常用到的是dataset，getAttribute，但是这个两个哪个效率更高那。

#getAttribute-vs-dataset
*转载需加来源*


```
    <div data-name='test' id='test'></div>
    <script type="text/javascript">
    window.onload = function() {
        var div = document.getElementById('test');
        console.log(div.dataset.name, div.getAttribute('data-name'))
        console.log(Object.prototype.toString.call(div.dataset), Object.prototype.toString.call(div.getAttribute))
    }
    </script>
    // test test
    // [object DOMStringMap] [object Function]
```
使用dataset操作data 要比使用getAttribute稍微慢些,但是如果我们只是处理少量的data数据，这点速度上差异造成的影响是基本可以忽略。使用dataset操作自定义
属性要比getAttribute的形式要少很多让人头疼的麻烦（例如：要获取多个值），并且更具有可读性。

以上面例子：
设置 div.dataset.name = 'test2'
删除 delete div.dataset.name
