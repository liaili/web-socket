# web-socket
## 做项目中用到web socket心得

    最近做的一个项目用到了web socket技术，那首先介绍下web socket的语法和用法吧，其实以下介绍有个坑，下面我会将解决方法如下：

>//第一步 接口地址     
var wsUrl = "ws://172.20.25.2:9000/video";      
var rtspUrl = "rtsp://172.20.25.3/user=admin&password=&channel=1&stream=0.sdp";     
var ws = wsUrl + "?url=" + encodeURIComponent(rtspUrl);

>//第二步  打开一个 web socket
    websocket = new WebSocket(ws);

>websocket.onopen=function(){
        // Web Socket 已连接上，使用 send() 方法发送数据,可有可无
        ws.send("发送数据");
}
>websocket.onmessage=function(evt){
  //接受接口数据
  var received_msg = evt.data;
}

>websocket.onclose = function(){ 
  // 关闭 websocket
};

网上有很多关于web socket的介绍，这里我就不多说了，我会说一些在开发过程中遇到的问题和发现的好用的地方，希望会对其他人有帮助。

好用的地方是：web socket技术似乎没有跨域问题，我在做项目的过程中，环境是会有跨域问题，之前开发还挺担心，因为第一次用这个技术，
但是我的代码并没有跨域问题的报错。

现在我就来说下坑到底在哪里，有写浏览器并不支持websocket.onmessage，websocket.onopen，websocket.onclose这样的写法，最起码
我之前在chrome上就不行，页面打开就直接执行了websocket.onclose了，但是正常情况是只执行websocket.onopen方法，靠谱的写法是如下：
websocket.addEventListener('open', function (e) {
  // Web Socket 已连接上，使用 send() 方法发送数据,可有可无
  ws.send("发送数据");
});
websocket.addEventListener('message', function (e) {
  //接受接口数据
  var received_msg = evt.data;
});
websocket.addEventListener('close', function (e) {
  // 关闭 websocket
});

以上就是我今天的笔记。







