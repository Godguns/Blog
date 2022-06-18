### 为什么会进行心跳检测   
简单地说是为了证明客户端和服务器还活着。websocket 在使用过程中，如果遭遇网络问题等，这个时候服务端没有触发onclose事件，这样会产生多余的连接，并且服务端会继续发送消息给客户端，造成数据丢失。
因此需要一种机制来检测客户端和服务端是否处于正常连接的状态，心跳检测和重连截止就产生了。
### 如何进行心跳检测和重连   
1.每隔一段指定的时间（计时器），向服务器发送一个数据，服务器收到数据后再发送给客户端，正常情况下客户端通过onmessage事件是能监听到服务器返回的数据的，说明请求正常。
2.如果再这个指定时间内，客户端没有收到服务器端返回的响应消息，就判定连接断开了，使用websocket.close关闭连接。
3.这个关闭连接的动作可以通过onclose事件监听到，因此在 onclose 事件内，我们可以调用reconnect事件进行重连操作。
### 实现   
```js    
$(function () {
    var path = basePath;
    var jspCode = $("#userId").val();
    var websocket;
    createWebSocket();

    /**
     * websocket启动
     */
    function createWebSocket() {
        try {
            if ('WebSocket' in window) {
                websocket = new WebSocket((path + "/wsCrm?jspCode=" + jspCode).replace("http", "ws").replace("https", "ws"));
            } else if ('MozWebSocket' in window) {
                websocket = new MozWebSocket(("ws://" + path + "/wsCrm?jspCode=" + jspCode).replace("http", "ws").replace("https", "ws"));
            } else {
                websocket = new SockJS(path + "/wsCrm/sockJs?jspCode=" + jspCode.replace("http", "ws"));
            }
            init();
        } catch (e) {
            console.log('catch' + e);
            reconnect();
        }

    }

    function init() {
        //连接成功建立的回调方法
        websocket.onopen = function (event) {
            console.log("WebSocket:已连接");
            //心跳检测重置
            heartCheck.reset().start();
        };

        //接收到消息的回调方法
        websocket.onmessage = function (event) {
            showNotify(event.data);
            console.log("WebSocket:收到一条消息", event.data);
            heartCheck.reset().start();
        };

        //连接发生错误的回调方法
        websocket.onerror = function (event) {
            console.log("WebSocket:发生错误");
            reconnect();
        };

        //连接关闭的回调方法
        websocket.onclose = function (event) {
            console.log("WebSocket:已关闭");
            heartCheck.reset();//心跳检测
            reconnect();
        };

        //监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
        window.onbeforeunload = function () {
            websocket.close();
        };

        //关闭连接
        function closeWebSocket() {
            websocket.close();
        }

        //发送消息
        function send(message) {
            websocket.send(message);
        }
    }

    //避免重复连接
    var lockReconnect = false, tt;

    /**
     * websocket重连
     */
    function reconnect() {
        if (lockReconnect) {
            return;
        }
        lockReconnect = true;
        tt && clearTimeout(tt);
        tt = setTimeout(function () {
            console.log('重连中...');
            lockReconnect = false;
            createWebSocket();
        }, 4000);
    }

    /**
     * websocket心跳检测
     */
    var heartCheck = {
        timeout: 5000,
        timeoutObj: null,
        serverTimeoutObj: null,
        reset: function () {
            clearTimeout(this.timeoutObj);
            clearTimeout(this.serverTimeoutObj);
            return this;
        },
        start: function () {
            var self = this;
            this.timeoutObj && clearTimeout(this.timeoutObj);
            this.serverTimeoutObj && clearTimeout(this.serverTimeoutObj);
            this.timeoutObj = setTimeout(function () {
                //这里发送一个心跳，后端收到后，返回一个心跳消息，
                //onmessage拿到返回的心跳就说明连接正常
                websocket.send("HeartBeat");
                console.log('ping');
                self.serverTimeoutObj = setTimeout(function () { // 如果超过一定时间还没重置，说明后端主动断开了
                    console.log('关闭服务');
                    websocket.close();//如果onclose会执行reconnect，我们执行 websocket.close()就行了.如果直接执行 reconnect 会触发onclose导致重连两次
                }, self.timeout)
            }, this.timeout)
        }
    };
});   
```
