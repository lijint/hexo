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

```
# 值类型传递
# 返回值传递

