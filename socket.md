# Socket

---

## 什么是Socket？Socket有哪些作用？

Socket又叫“套接字”。Socket是纯C语言，跨平台的。在应用层和传输层之间。网络通信其实就是Socket之间的通信。应用程序通过“套接字”向网络发送请求或向网络做出应答。数据在两个Socket之间通过I\/O传输数据。

Http协议是基于Socket的，HTTP协议的底层就是使用的Socket.

## 怎样使用Socket?

1. 客户端创建Socket
2. 客户端连接到服务器
3. 发送数据到服务器
4. 接收服务器回传的数据
5. 关闭连接

