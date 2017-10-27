---
title: Service的使用
date: 2017-08-08 17:23:51
tags: Android
---

> Service是Android的四大组件之一，它主要用于在后台处理一些耗时的逻辑，或者去执行某些需要长期运行的任务。必要的时候我们甚至可以在程序退出的情况下，让Service在后台继续保持运行状态。
<!--more-->

*******
## Service的基本用法

关于Service最基本的用法自然就是如何启动一个Service了，启动Service的方法和启动Activity很类似，都需要借助Intent来实现，下面我们就通过一个具体的例子来看一下。
新建一个Android项目，项目名就叫ServiceTest，这里我选择使用4.0的API。

然后新建一个MyService继承自Service，并重写父类的onCreate()、onStartCommand()和onDestroy()方法，如下所示：

```java
public class MyService extends Service {
    public static final String TAG = "MyService";  
    @Override  
    public void onCreate() {  
        super.onCreate();  
        Log.d(TAG, "onCreate() executed");  
    }
    @Override  
    public int onStartCommand(Intent intent, int flags, int startId) {  
        Log.d(TAG, "onStartCommand() executed");  
        return super.onStartCommand(intent, flags, startId);  
    }
    @Override  
    public void onDestroy() {  
        super.onDestroy();  
        Log.d(TAG, "onDestroy() executed");  
    }
    @Override  
    public IBinder onBind(Intent intent) {  
        return null;  
    } 
}
```
    
然后，只要在MainActivity中实现这个类，用Intent的方法启动和停止服务就可以了。其中MainActivity的函数如下：

```java
public class MainActivity extends Activity implements OnClickListener {
    private Button startService;
    private Button stopService;
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
        startService = (Button) findViewById(R.id.start_service);  
        stopService = (Button) findViewById(R.id.stop_service);  
        startService.setOnClickListener(this);  
        stopService.setOnClickListener(this);  
    }
    @Override  
    public void onClick(View v) {  
        switch (v.getId()) {  
        case R.id.start_service:  
            Intent startIntent = new Intent(this, MyService.class);  
            startService(startIntent);  
            break;  
        case R.id.stop_service:  
            Intent stopIntent = new Intent(this, MyService.class);  
            stopService(stopIntent);  
            break;  
        default:  
            break;  
        }  
    }
}  
```

布局中的代码就不写出来了，只有两个按钮，startService和stopService。

****
## Service与Activity的通信

Service与Activity的通信是通过绑定完成的，可以看到MyService类中，有个onBind(Intent intent)函数,上面返回的是null，如果要传递参数，则需要创建个内部类myBinder继承自Binder，然后在内部类里实现相关方法。随后在外部实现这个类，并将实现的myBinder作为onBind函数的返回值，返回给MainActivity使用。具体代码如下：

```java   
public class MyService extends Service {
    public MyBinder myBinder = new MyBinder();
    public static final String TAG = "MyService";
    public MyService() {
    }
    @Override
    public IBinder onBind(Intent intent) {
        return myBinder;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG, "OnCreate() executed");
    }
    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d(TAG, "onDestroy() executed");
    }
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d(TAG, "onStartCommand() executed");
        return super.onStartCommand(intent, flags, startId);
    }
    class MyBinder extends Binder {
        public void startDownload() {
            Log.d(TAG, "startDownload() executed.");
        }
    }
}
```

而Activity端呢，需要通过ServiceConnection匿名类来实现，具体操作是，先创建ServiceConnection匿名类，里面重写onServiceConnected()方法和onServiceDisconnected()方法，这两个方法分别会在Activity与Service建立关联和解除关联的时候调用。在onServiceConnected()方法中，我们又通过向下转型得到了MyBinder的实例，有了这个实例，Activity和Service之间的关系就变得非常紧密了。现在我们可以在Activity中根据具体的场景来调用MyBinder中的任何public方法，即实现了Activity指挥Service干什么Service就去干什么的功能。