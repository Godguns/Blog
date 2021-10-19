```javascript   
<body>
    <div id="a">
        <div id="b">
            <div id="c">
                <div id="d"></div>
            </div>
        </div>
    </div>
    
    <script>
        document.getElementById('a').addEventListener('click', function ( e ) {
            console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
        });
        document.getElementById('b').addEventListener('click', function ( e ) {
            console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
        });
        document.getElementById('c').addEventListener('click', function ( e ) {
            console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
        });
        document.getElementById('d').addEventListener('click', function ( e ) {
            console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
        });
    </script>
</body>
```   
> 处于冒泡阶段的时候    
  ```javascript   
      //点击d元素的时候；
    target:d&currentTarget:d    // d触发
    target:d&currentTarget:c    // c触发
    target:d&currentTarget:b    // b触发
    target:d&currentTarget:a    // a触发
```   
从输出中我们可以看到，event.target指向引起触发事件的元素，而event.currentTarget则是事件绑定的元素，只有被点击的那个目标元素的event.target才会等于event.currentTarget。
将上述<script>标签里的事件绑定的第三个参数设置为true时，事件处于捕获阶段，然后还是点击最里层的元素d，这时event.target还依旧会指向d，因为那是引起事件触发的元素，因为event.currentTaget指向事件绑定的元素，所以在捕获阶段，最先来到的元素是a,然后是b,接着是c,最后是d。所以输出的内容如下：   
 
》处于捕获阶段   
  ```   
      target:d&currentTarget:a    // a触发
    target:d&currentTarget:b    // b触发
    target:d&currentTarget:c    // c触发
    target:d&currentTarget:d    // d触发
```   
  
