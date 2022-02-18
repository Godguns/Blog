### DOMContentLoaded   
当纯HTML被完全加载以及解析时，DOMContentLoaded 事件会被触发，而不必等待样式表，图片或者子框架完成加载。比下面其他几个方法都先执行   
```javascript   
document.addEventListener('DOMContentLoaded', () => {
console.log('DOMContentLoaded');
});   
```   
### window.onload   
页面内容全部加载完后执行   
```javascript   
$(window).on('load',function () {
    console.log('$onload');
});
window.onload = function () {//只能有一个，多个也只执行一个
    console.log('onload');
}
```   
* 1）解析HTML结构
* 2）加载外部的脚本和样式文件
* 3）解析并执行脚本代码
* 4）执行$(function(){})内对应代码
* 5）加载图片等二进制资源
* 6）页面加载完毕，执行window.onload
