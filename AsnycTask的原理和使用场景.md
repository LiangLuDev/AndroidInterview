### AsyncTask
由`Android`封装的一个轻量级异步类（轻量体现在使用方便、代码简洁），它的内部封装了两个线程池(`SerialExecutor`和`THREAD_POOL_EXECUTOR`)和一个`Handler(InternalHandler)`，其中`SerialExecutor`线程池用于任务的排队，`THREAD_POOL_EXECUTOR`线程池才真正地执行任务，`InternalHandler`用于从工作线程切换到主线程。

### 使用场景
- 适合：耗时时间较短的任务，HTTP 请求
- 不适合：大文件下载和数据库更改。因为会导致线程堵塞，没有线程来执行其他的任务。
 

### Handler+Thread比较
#### AsyncTask
- **优点**
> 1.使用简单，代码简洁；
>
> 2.过程可控，可调用`cancel()`取消任务。

- **缺点**
> 执行多个异步操作并需要更新UI时就变得复杂起来。

#### Handler+Thread

- **优点**
> 1.代码结构清晰，功能定义明确；
>
> 2.执行多个异步操作时，结构清晰，功能明确。

- **缺点**
> 1.执行单一异步处理时，实现起来相对来说代码过多，结构复杂。
>
> 2.AsyncTask比Handler更轻量级一些（只是代码上轻量一些，而实际上要比handler更耗资源）


### 方法说明

- `onPreExecute()`:此方法会在后台任务执行前被调用，用于进行一些准备工作
- `doInBackground(Params... params)`:此方法中定义要执行的后台任务，在这个方法中可以调用publishProgress来更新任务进度（publishProgress内部会调用onProgressUpdate方法）
- `onProgressUpdate(Progress... values)`:由publishProgress内部调用，表示任务进度更新
- `onPostExecute(Result result)`:后台任务执行完毕后，此方法会被调用，参数即为后台任务的返回结果
- `onCancelled()`:此方法会在后台任务被取消时被调用

### AsnyncTask原理
AsyncTask主要有二个部分：一个是与主线程的交互，另一个就是线程的管理调度。虽然可能多个AsyncTask的子类的实例，但是AsyncTask的内部Handler和ThreadPoolExecutor都是进程范围内共享的，其都是static的，也即属于类的，类的属性的作用范围是CLASSPATH，因为一个进程一个VM，所以是AsyncTask控制着进程范围内所有的子类实例。　

AsyncTask内部会创建一个进程作用域的线程池来管理要运行的任务，也就就是说当你调用了AsyncTask的execute()方法后，AsyncTask会把任务交给线程池，由线程池来管理创建Thread和运行Therad。
