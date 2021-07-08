在调用document.write时，会将内容写入文档流，所以会自动调用document.open方法打开文档流，如果文档流已经关闭，则会清除所有内容。
那什么时候代表文档流已经关闭呢？即所有的内容都已经读入到浏览器中(含所有的内联CSS、HTML标签)，即页面的主线程没有执行完成，也就是我们所看到的源代码里面的所有内容。
### 如下列代码
```javascript   
<body>
    <h1>Hello, World!</h1>
    <script type="text/javascript">
        document.write('<h2>Hello, Yiifaa!</h2>')
    </script>
</body>
```   
因为此时页面的主线程没有执行完成，所以会将“   

Hello, Yiifaa!   

”添加到文档末尾，但如果利用setTimeout暂时放弃主线程的执行，切换到任务队列中，那么随着文档流的写入结束，此时需要重新打开文档流，那么文档的内容则会全部覆盖，如下：      
```javascript   
<body>
    <h1>Hello, World!</h1>
    <script type="text/javascript">
        //  暂时放弃主线程的执行
        setTimeout(function() {
            document.write('<h2>Hello, Yiifaa!</h2>')
        }, 0);
    </script>
</body>
```   
通过上面的例子可以看出，只要浏览器解析到文档末尾，则浏览器主线程结束，文档流关闭。   

需要特别指出的，文档是严格按照从下直下的顺序执行的，即使某一个js由于网络原因延迟了较长时间，那么文档也会一直等待它的执行，直到超时或等待的JS执行完成，从而将浏览器主线程的执行周期拉长
