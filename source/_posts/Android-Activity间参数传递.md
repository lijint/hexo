---
title: Android Activity间参数传递
date: 2018-08-06 10:36:59
tags: Android
---

>Android中的Activity间的数据传递在实际开发中是非常实用的，主要分为以下几部分：简单的数据传递、传递Bundle、值类型传递、返回值传递，这四种情况
<!--more-->

Activity之间的数据传递是通过Intent来实现的，将数据放入Intent中，传递给目标Activity

# 简单数据传递
简单的数据类型通常比较单一，例如int、string，所以，实现起来也比较简单，首先，我们先在要传出参数的Activity中，将参数放进Intent中
```java
	protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_main);
	    textView = findViewById(R.id.tv);
	    findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
	        @Override
	        public void onClick(View view) {
	            Intent intent = new Intent(MainActivity.this, AnotherActivity.class);
	            intent.putExtra("myage", 18);
	            intent.putExtra("myname", "lijint");
	            startActivity(intent);
	        }
	    });
	}
```
在接收端，我们先使用getIntent()来获取Intent，然后在取出Intent里的数据
```java
	protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_another);
	    editText = findViewById(R.id.editText);
	    tv = findViewById(R.id.textView);
	    Intent i = getIntent();
	    editText.setText(String.format("name : %s  age : %d", i.getStringExtra("myname"),
	            i.getIntExtra("myage",0)));
```

# 传递Bundle数据
在实际使用过程中，需要传递的参数往往很多，如果都是以单一的传递方式传的话效率低下，且不便管理，因此，Android引入了Bundle,将数据放进Bundle里，在进行统一的传递。

直接上代码吧，数据发送端：
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = findViewById(R.id.tv);

        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                Intent intent = new Intent(MainActivity.this, AnotherActivity.class);

                Bundle bundle = new Bundle();
                bundle.putString("myname", "lijint");
                bundle.putInt("myage", 18);
                intent.putExtras(bundle);
                startActivity(intent);
            }
        });
    }
```

接收端接到Intent后，取出里面的Bundle，在从里面取出数据
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_another);
        editText = findViewById(R.id.editText);
        tv = findViewById(R.id.textView);
        Intent i = getIntent();
        Bundle bundle = i.getExtras();

        editText.setText(String.format("name : %s  age : %d", bundle.getString("myname"),
                bundle.getInt("myage")));
    }

```

# 值类型传递

所谓的值传递，就是将一种自定义的形式组成的数据传递出去的一种方式，通常是传递一个对象，这个对象是由我们来定义，通常传递值类型有两种做法，一种是使用java自带的Serializable操作，这种写法比较简单，但比较耗内存；另一种是使用android封装的Parcelable，这种写法相对复杂，但比较高效。下面来详细介绍这两种写法：

## Serializable
首先我们先创建一个类，实现Serializable这个接口
```java
public class User implements Serializable {

    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
然后发送端的程序很简单，intent.putExtra()函数重载了多个参数，既可以传单个变量，也可以传Bundle，还能传自定义的类型，真是太方便了
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = findViewById(R.id.tv);

        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, AnotherActivity.class);
                intent.putExtra("user", new User("lijint", 18));
                startActivity(intent);
            }
        });
    }
```

在接收端，我们要通过intent，来获取放在里面的自定义的值，这里取出的函数稍微不同，使用的是**getSerializableExtra()**
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_another);
        editText = findViewById(R.id.editText);
        tv = findViewById(R.id.textView);
        Intent i = getIntent();

        User user = (User) i.getSerializableExtra("user");
        tv.setText(String.format("name : %s  age : %d", user.getName(), user.getAge()));

    }

```


## Parcelable
同样，我们先建一个类，与上一个不同的是这次实现Parcelable接口，在这个类中，我们必须实现几个方法和一个Creator，其中writeToParcel的作用是将你获取到的数据放进Android Parcel里，describeContents方法没啥卵用，Creator则是你要从parcel获取数据时用到的，将Parcel里的数据取出来，放进新的类里面

```java
public class User implements Parcelable {

    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel parcel, int i) {
        parcel.writeString(getName());
        parcel.writeInt(getAge());
    }

    public static final Creator<User> CREATOR = new Creator<User>() {
        @Override
        public User createFromParcel(Parcel parcel) {
            return new User(parcel.readString(),parcel.readInt());
        }

        @Override
        public User[] newArray(int i) {
            return new User[i];
        }
    };
}

```

在发送端，由于函数的超强重载性，使用一个putExtra()就能实现所有传递，因此，发送端的程序没有变化，但在接收端，我们必须使用专门接收Parcelable的函数进行接收

```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_another);
        editText = findViewById(R.id.editText);
        tv = findViewById(R.id.textView);
        Intent i = getIntent();

        User user = i.getParcelableExtra("user");
        tv.setText(String.format("name : %s  age : %d", user.getName(), user.getAge()));

    }

```

# 返回值传递
我们将刚才讨论的第一个Activity称为Aty1，调起的Activity称为Aty2，前面我们讨论的是从Aty1传递数据到Aty2的情况，那么，要怎么将数据从Aty2传回Aty1呢，这时候就要用到返回值了。
这里需要注意到的是，要使用返回值传递，Aty1的调用函数就不能使用原本的了，并且发送端还需要实现一个接受返回回来数据的函数onActivityResult
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = findViewById(R.id.tv);

        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                Intent intent = new Intent(MainActivity.this, AnotherActivity.class);

                intent.putExtra("user", new User("lijint", 18));

                startActivityForResult(intent, 1);
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == 1 && resultCode == 1) {
            textView.setText("另一个Activity返回的数据是 ： " + data.getStringExtra("data"));
        }
    }

```

Aty2程序
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_another);
        editText = findViewById(R.id.editText);
        tv = findViewById(R.id.textView);
        Intent i = getIntent();

        button = findViewById(R.id.btnSendBack);

        User user = i.getParcelableExtra("user");
        tv.setText(String.format("name : %s  age : %d", user.getName(), user.getAge()));

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent i = new Intent();
                i.putExtra("data", editText.getText().toString());
                setResult(1, i);
                finish();
            }
        });

```
同样，传递的数据类型可以是多种，都是使用intent这个载体进行传递。

总结，Android的数据传递使用非常方便，主要是使用intent这个载体，往里面塞数据，并且android提供了bundle这个类型，使得数据传递更具一体性，Android还提供Parcelable接口，能很快速的将值类型序列化。

