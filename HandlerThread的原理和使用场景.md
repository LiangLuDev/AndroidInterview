
### HandlerThread
HandlerThread是Thread的一个子类，HandlerThread自带Looper使他可以通过消息队列来重复使用当前线程，节省系统资源开销。这是它的优点也是缺点，每一个任务都将以队列的方式逐个被执行到，一旦队列中有某个任务执行时间过长，那么就会导致后续的任务都会被延迟处理


能够新建拥有Looper的线程。这个Looper能够用来新建其他的Handler。（线程中的Looper）需要注意的是，新建的时候需要被回调。


HandlerThread继承于Thread，所以它本质就是个Thread。与普通Thread的差别就在于，主要的作用是建立了一个线程，并且创立了消息队列，有来自己的looper,可以让我们在自己的线程中分发和处理消息。




### 为什么使用HandlerThread？

`Handler+Thread`需要我们自己操作Looper，所以Google封装了HandlerThread,类似封装还有`AsyncTask`
