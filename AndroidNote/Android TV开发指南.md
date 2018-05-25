### Android TV Development Guide
除了Phone和Table外，安卓还广泛地应用智能电视，智能穿戴，车载系统等。品牌电视厂商纷纷转型智能电视，Android TV是通过红外遥控器操作，部分支持手势，语音。这和手机开发是有一点差异的，手势操作，更加丰富，遥控操作，局限性很大。

在新建项目上，Phone/Table、TV、Wear等项目结构相同，唯一的区别是TV、Wear有提供支持库，所以我们新建项目可以随意建成手机应用的项目。

Android TV开发最核心的问题是**焦点控制**、**AndroidTVWidget使用**、**测试调式**

#### 焦点控制
Android TV是遥控操作，遥控常有的功能按键有，数字键、上下左右方向键，确定返回键等，上下左右控制选项，确定键控制点击。系统会从上往下，从左往右传递焦点。

###### 常用功能

- 不同于手机，我们需要将我们的控件设置焦点，否则将无法上下左右选中
  ```java
  android:focusable="true"
  android:focusableInTouchMode="true"
  ```

- 设置焦点了，还需要焦点选择中效果，新建一个selector,设置state_focused，然后给设置背景即可
  ```java
  <?xml version="1.0" encoding="utf-8"?>
  <selector xmlns:android="http://schemas.android.com/apk/res/android" >
      <item android:drawable="@drawable/item_focus" android:state_focused="true"/>
      <item android:drawable="@android:color/transparent"  android:state_focused="false"/>
      <item android:drawable="@android:color/transparent"/>
  </selector>
  ```

- 监听控件焦点变化
```java
setOnFocusChangeListener(new View.OnFocusChangeListener() {
            @Override
            public void onFocusChange(View view, boolean hasFocus) {
                //hasFocus当前控件是否有焦点
            }
        });
```

###### 特殊要求
当我们的有一个列表五个按钮吧，当我们在第一个按钮处，遥控按"上键",此时没有任何反应，我们希望第一个按上键，能把焦点传递给最后一个。反之，最后一个按"下键"将焦点传递给最上面的按钮。（参数就是id）
```java
android:nextFocusUp=""
android:nextFocusDown=""
android:nextFocusLeft=""
android:nextFocusRight=""
```

#### AndroidTVWidget使用


