---
title: LayoutInflater使用
date: 2017-08-09 10:08:37
tags: Android
---

>android编码中常常会使用到需要动态的加载View的情况，这时候我们就需要使用到LayoutInflate。

<!--more-->

## 基本用法

首先先实例化一个LayoutInflater,有两种写法：

```java
LayoutInflater layoutInflater = LayoutInflater.from(context);
LayoutInflater layoutInflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
```

明显第一种是第二种的简化版，android给我们加了一些封装。
接下来调用实例化来的LayoutInflater的inflate()方法：

```java
layoutInflater.inflate(resourceId,root);
```

inflate()方法一般接收两个参数，第一个是要加载的布局id，第二个是指给该布局的外部再嵌套一层父布局，如果不需要就直接传null。这样就成功成功创建了一个布局的实例，之后再将它添加到指定的位置就可以显示出来了。

举个例子吧，我们先创建个原始的布局文件,MainActivity对应的activity_main.xml:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:id="@+id/main_layout"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent" >
</LinearLayout>
```

然后再建一个button_activity.xml：

```xml
<Button xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:text="Button" >
</Button>
```

最后在MainActivity里把button布局加进去：

```java
public class MainActivity extends Activity {  
    private LinearLayout mainLayout;
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
        mainLayout = (LinearLayout) findViewById(R.id.main_layout);  
        LayoutInflater layoutInflater = LayoutInflater.from(this);  
        View buttonLayout = layoutInflater.inflate(R.layout.button_layout, null);  
        mainLayout.addView(buttonLayout);  
    }
}
```
运行后的结果：
![图片标题](https://leanote.com/api/file/getImage?fileId=596f0dd5ab64411940000fe6)


出处：[Android LayoutInflater原理分析，带你一步步深入了解View(一)](http://blog.csdn.net/guolin_blog/article/details/12921889)