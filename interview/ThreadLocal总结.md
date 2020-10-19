# ThreadLocal总结

- ThreadLocal作用：为每一个线程开辟一个副本，意思是线程本地

### 1、代码证明ThreadLocal为每个线程创建线程
```xml
线程==>
public class ThreadDemo extends Thread {

    ThreadLocal<Long> threadLocal;

    public ThreadDemo(ThreadLocal<Long> threadLocal) {
        this.threadLocal = threadLocal;
    }

    @Override
    public void run() {
        Long result = threadLocal.get();
        if (result == null) {
            threadLocal.set(System.currentTimeMillis());
            System.out.println(Thread.currentThread().getName() + "===>" + threadLocal.get()+"===>"+threadLocal);
        }
    }
}


证明==>

public class ThreadLocalDemo {

    public static ThreadLocal<Long> threadLocal = new ThreadLocal();

    public static void main(String[] args) {
        ThreadDemo t1 = new ThreadDemo(threadLocal);
        ThreadDemo t2 = new ThreadDemo(threadLocal);
        t1.start();
        try { Thread.sleep(100); } catch (InterruptedException e) { e.printStackTrace(); }
        t2.start();

    }

}

```


### 2、为什么为线程创建副本：分析ThreadLocal的get/set方法
```xml
set=====>

public void set(T value) {
    //获取当前线程
    Thread t = Thread.currentThread();
    //获得当前线程的成员变量，类型为ThreadLocal.ThreadLocalMap
    ThreadLocalMap map = getMap(t);
    if (map != null)
        //key:this，就是当前线程；value：就是存入的泛型值
        map.set(this, value);
    else
        createMap(t, value);
}

get ====>

public T get() {
        //获取当前线程
        Thread t = Thread.currentThread();
        //获得当前线程的成员变量，类型为ThreadLocal.ThreadLocalMap
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }


```
### 3、实际解决了什么？

```xml
mybatis内部sqlsession其内部就是采用ThreadLocal

爬虫每个Request的代理利用ThreadLocal

```

### 4、产生了什么问题？

```xml
内存泄露；
比如线程池核心线程之外的线程执行完任务，但是ThreadLocal没有释放，就会导致内存泄露
解决：可以通过delete()删除

```