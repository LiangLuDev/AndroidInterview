### Handler
- Handler封装了消息的发送，也负责接收消。内部会跟Looper关联。
- Looper 内部包含了MessageQueue，负责从MessageQueue取出消息，然后交给Handler处理
- MessageQueue 就是一个消息队列，负责存储消息，有消息过来就存储起来，Looper会循环的从MessageQueue读取消息。


引用鸿洋大神的一张图片，一目了然：

![Handler](https://img-blog.csdn.net/20140805002935859?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbG1qNjIzNTY1Nzkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


