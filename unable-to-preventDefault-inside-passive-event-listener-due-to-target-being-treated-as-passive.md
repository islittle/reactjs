>被动事件监听器是一种新兴的Web标准，Chrome 51中提供了新功能，为滚动性能提供了重要的潜在优势。 Chrome发行说明。它使开发人员能够选择更好地滚动性能，因
为消除了滚动以阻止触摸和滚轮事件监听器的需要

##unable-to-preventDefault-inside-passive-event-listener-due-to-target-being-treated-as-passive

所有现代浏览器都有一个线程滚动功能，允许滚动运行顺利，即使昂贵的JavaScript正在运行，但这种优化被部分打败了需要等待任何touchstart和touchmove处理程序
的结果，这可能会阻止滚动完全通过调用preventDefault（）对事件。

解决方案： - {passive：true}

通过将触摸或滚轮监听器标记为被动，开发人员承诺，处理程序不会调用preventDefault禁用滚动。这样就可以让浏览器自动回滚滚动，而不用等待JavaScript，从而
确保用户可以平滑滚动的体验。

```
addEventListener(document, "touchstart", function(e) {
    console.log(e.defaultPrevented);  // will be false
    e.preventDefault();   // does nothing since the listener is passive
    console.log(e.defaultPrevented);  // still false
  }, Modernizr.passiveeventlisteners ? {passive: true} : false);
```

还有一种css方法：touch-action: none;
