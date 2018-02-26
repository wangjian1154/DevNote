### 动画
---
- ***Fragment设置进场动画可以通过setCustomAnimations方法进行设置,虽然有两个参数，但是这是无效的。两个参数的方法只能设置进场动画***
```java
 transaction.setCustomAnimations(R.anim.category_enter, R.anim.category_exit);
```
退出动画需要4个参数的方法，主要参数就是参数1和参数4。
```java
transaction.setCustomAnimations(R.anim.category_enter, R.anim.category_exit, R.anim.category_enter, R.anim.category_exit)//设置退出动画
```

- ***页面从上往下的动画***
```java
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/accelerate_interpolator"
    android:fromYDelta="-100%p"
    android:toYDelta="0%p"
    android:duration="600">
</translate>
```
- ***页面从下往上的动画***
```java
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/accelerate_interpolator"
    android:fromYDelta="-100%p"
    android:toYDelta="0%p"
    android:duration="600">
</translate>
```

***
### Style
***
