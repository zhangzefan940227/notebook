代码均使用Kotlin编写。

# 通过xml获取菜单列表项

```kotlin
/**
 * 通过xml文件获取菜单列表项
 * @param xml 传入的xml文件名称，包含后缀
 *
 */
@JvmStatic
@Throws(IOException::class, XmlPullParserException::class)
fun getMenuList(xml: InputStream?): List<MenuModel> {
    val menuList: MutableList<MenuModel> = ArrayList()
    var menuModel: MenuModel? = null
    val pullParser = Xml.newPullParser()
    try {
        //为PULL解析器设置要解析的XML数据
        pullParser.setInput(xml, "UTF-8")
        var event = pullParser.eventType
        while (event != XmlPullParser.END_DOCUMENT) {
            // when 等价于 switch
            when (event) {
                XmlPullParser.START_TAG -> {
                    when (pullParser.name) {
                        ITEM -> { menuModel = MenuModel() }
                        TITLE -> { menuModel?.setTitle(pullParser.nextText()) }
                        JUMP_TO_WHERE -> {
                            menuModel?.setJumpToWhere(Class.forName(pullParser.nextText()))
                        }
                    }
                }
                XmlPullParser.END_TAG -> {
                    if (ITEM == pullParser.name && menuModel != null) {
                        menuList.add(menuModel)
                        menuModel = null
                    }
                }
            }
            event = pullParser.next()
        }
    } catch (e: XmlPullParserException) {
        e.printStackTrace()
    } catch (e: ClassNotFoundException) {
        throw RuntimeException(e)
    }
    LogUtils.i(tag, menuList.toString())
    return menuList
}

```

# 线程池

## Web请求模拟

模拟一个简单的Web服务器，接受请求并使用线程池进行处理。

```java
import java.util.concurrent.*;

public class SimpleWebServer {
    private static final int CPU_COUNT = Runtime.getRuntime().availableProcessors();
    private static final int CORE_POOL_SIZE = 2 * CPU_COUNT;
    private static final int MAX_POOL_SIZE = 2 * CPU_COUNT + 1;

    private static final ThreadPoolExecutor exec = new ThreadPoolExecutor(
            CORE_POOL_SIZE,
            MAX_POOL_SIZE,
            60L,
            TimeUnit.SECONDS,
            new LinkedBlockingQueue<>(1000)
    );

    public static void main(String[] args) {
        while (true) {
            Runnable request = () -> System.out.println("Request handled by " + Thread.currentThread().getName());

            exec.execute(request);
        }
    }
}
```

## 并行计算

并行计算的结果通过调用 exec.submit(task) 获取

```java
import java.util.concurrent.*;

public class ParallelCalculation {

    private static final int NTHREADS = 4;
    private static final ExecutorService exec = Executors.newFixedThreadPool(NTHREADS);

    public static void main(String[] args) {
        Callable<Double> task = new Callable<Double>() {
            @Override
            public Double call() {
                // 这里模拟一些数值计算
                return Math.random() * 100;
            }
        };

        List<Future<Double>> results = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            results.add(exec.submit(task));
        }

        for (Future<Double> result : results) {
            try {
                System.out.println(result.get());
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }

        exec.shutdown();
    }
}
```

## 模拟异步处理

```java
import java.util.concurrent.*;

public class AsynchronousTaskProcessor {

    private static final ExecutorService exec = Executors.newCachedThreadPool();

    public static void main(String[] args) {
        exec.execute(() -> {
            // 执行某些异步任务
            System.out.println("Async task started");
            try {
                // 模拟耗时2s的异步操作
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Async task completed");
        });

        System.out.println("Main thread continues to execute other operations.");
        exec.shutdown();
    }
}
```

# IO流文件读写


# Android中的单例模式

以AMS中的getService为例，使用抽象类Singleton作为单例类，子类进行初始化时需要重写create()方法，实例化在create()中进行。通过get()方法获取实例化对象
```java
    @UnsupportedAppUsage
    public static IActivityManager getService() {
        return IActivityManagerSingleton.get();
    }

    private static final Singleton<IActivityManager> IActivityManagerSingleton =
            new Singleton<IActivityManager>() {
                @Override
                protected IActivityManager create() {
                    final IBinder b = ServiceManager.getService(Context.ACTIVITY_SERVICE);
                    final IActivityManager am = IActivityManager.Stub.asInterface(b);
                    return am;
                }
            };

```

IActivityManagerSingleton 接口代码
```java
public abstract class Singleton<T> {

    @UnsupportedAppUsage
    public Singleton() {
    }

    @UnsupportedAppUsage
    private T mInstance;

    protected abstract T create();

    @UnsupportedAppUsage
    public final T get() {
        synchronized (this) {
            if (mInstance == null) {
                mInstance = create();
            }
            return mInstance;
        }
    }
}

```