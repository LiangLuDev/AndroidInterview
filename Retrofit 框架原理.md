## Retrofit
> 针对OkHttp封装的网路框架，使用注解的方式开发，省去大量创建类似的请求与方法，

## 原理
- [动态代理](http://www.codekk.com/blogs/detail/54cfab086c4761e5001b253d):
 > 动态代理可以理解成是双层静态代理，表面是A代理了B，其实是A将调用转发给C，C再将调用转发给B。
 开发者需要提供一个实现了InvocationHandler的子类 C。用户直接调用代理类 A 的对象，A 将调用转发给委托类 C，委托类 C 再将调用转发给它的委托类 B。 

- 注解:
> 请求方式、请求参数；



## 解析
> 跟着代码看解析吧

```
public <T> T create(final Class<T> service) {
     Utils.validateServiceInterface(service);
     if (validateEagerly) {
       eagerlyValidateMethods(service);
     }
     //这里返回一个 service 的代理对象
     return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
         new InvocationHandler() {
           private final Platform platform = Platform.get();

           @Override 
           public Object invoke(Object proxy, Method method, Object... args) throws Throwable {
             // If the method is a method from Object then defer to normal invocation.
             if (method.getDeclaringClass() == Object.class) {
               return method.invoke(this, args);
             }
             //DefaultMethod 是 Java 8 的概念，是定义在 interface 当中的有实现的方法
             if (platform.isDefaultMethod(method)) {
               return platform.invokeDefaultMethod(method, service, proxy, args);
             }
             //每一个接口最终实例化成一个 ServiceMethod，并且会缓存
             ServiceMethod serviceMethod = loadServiceMethod(method);

             //由此可见 Retrofit 与 OkHttp 完全耦合，不可分割
             OkHttpCall okHttpCall = new OkHttpCall<>(serviceMethod, args);
             //下面这一句当中会发起请求，并解析服务端返回的结果
             return serviceMethod.callAdapter.adapt(okHttpCall);
           }
         });
    }

```




`Retrofit`对象的`create()`方法，其内部实现就是通过`Porxy.newProxyInstance()`来创建代理对象,就可以在代理对象身上去调用的接口定义的那些方法了。

代理对象可以对原本委托对象的方法进行增强，附加更多的功能。而对于`Retrofit`来讲，代理对象就是给原本委托对象中相应的方法增加网络请求获取的功能，这个功能具体由谁来做呢，由`OkHttp`来做。


Retrofit 为我们构造了一个 OkHttpCall ，实际上每一个 OkHttpCall 都对应于一个请求，它主要完成最基础的网络请求，而我们在接口的返回中看到的 Call 默认情况下就是 OkHttpCall 了，如果我们添加了自定义的 callAdapter，那么它就会将 OkHttp 适配成我们需要的返回值，并返回给我们。

