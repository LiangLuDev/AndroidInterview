### MVC架构
Model View Controller，是软件架构中最常见的一种框架，简单来说就是通过controller的控制去操作model层的数据，并且返回给view层展示，具体见下图:
![MVC](http://www.jcodecraeer.com/uploads/20160414/1460565635729862.png)

当用户出发事件的时候，view层会发送指令到controller层，接着controller去通知model层更新数据，model层更新完数据以后直接显示在view层上，这就是MVC的工作原理。

但是在Android里面，造成了Activity既是controller层，又是view层的这样一个窘境

**缺点：** View可以与Model直接交互，导致两层之间耦合性太强。


### MVP架构
MVP作为MVC的演化，解决了MVC不少的缺点，对于Android来说，MVP的model层相对于MVC是一样的，而activity和fragment不再是controller层，而是纯粹的view层，所有关于用户事件的转发全部交由presenter层处理。看图说话：
![MVP](http://www.jcodecraeer.com/uploads/20160414/1460565637114968.png)

从图中就可以看出，最明显的差别就是view层和model层不再相互可知，完全的解耦，取而代之的presenter层充当了桥梁的作用，用于操作view层发出的事件传递到presenter层中，presenter层去操作model层，并且将数据返回给view层，整个过程中view层和model层完全没有联系。看到这里大家可能会问，虽然view层和model层解耦了，但是view层和presenter层不是耦合在一起了吗？其实不是的，对于view层和presenter层的通信，我们是可以通过接口实现的，具体的意思就是说我们的activity，fragment可以去实现实现定义好的接口，而在对应的presenter中通过接口调用方法。不仅如此，我们还可以编写测试用的View，模拟用户的各种操作，从而实现对Presenter的测试。这就解决了MVC模式中测试，维护难的问题。

**推荐使用：** MVP架构也是Android目前使用最多的架构。

### MVVM架构
MVVM的目标和思想MVP类似，利用数据绑定(DataBinding)、依赖属性(Dependency Property)、命令(Command)、路由事件(Routed Event)等新特性，打造了一个更加灵活高效的架构。
具体可以看图：
![MVVM](http://www.jcodecraeer.com/uploads/20160414/1460565638651974.png)

- View层不做任何业务逻辑、不涉及操作数据、不处理数据、UI和数据严格的分开。
- ViewModel层做的事情刚好和View层相反，ViewModel 只做和业务逻辑和业务数据相关的事，不做任何和UI、控件相关的事，ViewModel 层不会持有任何控件的引用，更不会在ViewModel中通过UI控件的引用去做更新UI的事情。
- Model 的职责很简单，基本就是实体模型（Bean）同时包括Retrofit 的Service ，ViewModel 可以根据Model 获取一个Bean的Observable<Bean>( RxJava ),然后做一些数据转换操作和映射到ViewModel 中的一些字段，最后把这些字段绑定到View层上。


**可以尝试：** DataBinding对Android Studio支持不怎么好，不怎么好调错。