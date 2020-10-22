websocket\http2\comet都能完成服务器推送，他们有什么区别呢？

[Comet：基于 HTTP 长连接的“服务器推”技术](https://www.ibm.com/developerworks/cn/web/wa-lo-comet/)
[websocket与comet的性能对比](https://chenkangxian.iteye.com/blog/2268133)
[客户端与服务器持续同步解析（轮询，comet，WebSocket）](https://www.cnblogs.com/pannysp/archive/2012/02/10/2346084.html)
[使用 WebSocket 和 SSE 实现 HTTP 服务器推送](https://www.ibm.com/developerworks/cn/web/wa-http-server-push-with-websocket-sse/index.html)
[HTTP/2 幕后原理](https://www.ibm.com/developerworks/cn/web/wa-http2-under-the-hood/index.html)

不管是Comet的何种方式(1、基于AJAX和基于IFrame的流（streaming）方式；2、基于AJAX的长轮询（long-polling）方式），其实都只是单向通信，直到WebSocket的出现，才是B/S之间真正的全双工通信