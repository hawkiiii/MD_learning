#### BitCoinServer

用注解@ServerEndpoint("/ws/bitcoinServer")把它标记为一个WebSocket Server

**OnOpen** 表示有浏览器链接过来的时候被调用
**OnClose** 表示浏览器发出关闭请求的时候被调用
**OnMessage** 表示浏览器发消息的时候被调用
**OnError** 表示有错误发生，比如网络断开了等等
**sendMessage** 用于向浏览器回发消息

#### ServerManager

维护了一个线程安全的集合servers, 用于因为浏览器发起连接请求而创建的BitCoinServer，broadCast 方法遍历这个集合，让每个Server向浏览器发消息

#### BitCoinDataCenter

继承HttpServlet，标记为Servlet不是为了其被访问，而是为了便于伴随Tomcat一起启动