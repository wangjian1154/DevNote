# 热门Android面试题

### 事件分发机制
- 整个view事件触发的流程是：
```
View.dispatchTouchEvent--->View.setOnTouchListener--->View.onTouchEvent
```
>在dispatchTouchEvent中对onTouchListener进行判断，看其返回值是否为null,如果不为null，就返回true，说明事件已经被消费掉。
<br>
<br>
setOnTouchListener,onTouchEvent都不会被执行。如果返回为false，那么就要看setOnTouchListener的返回值是否为false，
<br>
<br>
如果返回为false，则onTouchEvent就会被执行，否则，onTouchEvent就不会被执行。
onLongclicklistener执行的流程，在onTouchEvent被执行后，返回值为false，在115ms-500ms之间会执行onLongclicklistener
<br>
<br>
onClickListener的执行流程，当onLongclicklistener返回值为true，onclicklistener就不会被调用。

- ViewGroup+view：
执行流程：
```
MyLinearLayout.dispatchTouchEvent-->MyLinearLayout.onInterceptTouchEvent-->MyButton.dispatchTouchEvent-->MyButton.onTouchEvent
```
>dispatchTouchEvent:
执行顺序：由父控件到子控件的分发。
不管viewgroup是否被消费，每次执行事件的时候都会执行分发的方法。
<br>
<br>
onInterceptTouchEvent:
只有viewgroup才有的方法，负责拦截。
返回值：
返回为true：表示拦截该事件，不想子view中分发，自己处理
返回为false：表示不拦截，事件向子view中传递。
<br>
<br>
onTouchEvent :由最深层的子控件开始处理这个方法,选择是否消费这个事件。
返回值：
false：此控件不能消费这个事件，直接向上回传，直到找到能处理他的事件或者无控件处理，事件被作废。
true: 此控件负责处理这个事件。
<br>
<br>

- 事件分发的流程：
>首先会手势在屏幕上传递事件，会先走到最外层的布局的dispatchTouchEvent--》然后通过onInterceptTouchEvent的返回值，
判断是否拦截，如果拦截，就不走子视图当中的事件响应了，而执行自己的onTouchEvent方法，如果不拦截，就会走子视图
dispatchTouchEvent--onTouch--onTouchEvent方法，如果onTouchEvent的返回值为true，那么这个事件此子视图负责消费，
否则就会回传到上一视图的onTouchEvent当中来处理，如果上一视图返回为true，则其消费，否则事件回收不做处理。
<br>
在onTouchListener当中的onTouch方法里，判断返回值，然后决定是否执行onTouchEvent方法，然后在onTouchEvent方法
的ACTION_DOWN操作中通过判断时间和标志位，决定是否执行onLongClickListener当中onLongClick操作，然后在onLongClick
方法中通过判断返回值，来确定onClickListener的onClick方法是否会执行，如果返回false，都能执行，如果返回true，
onClick不执行。

### 消息处理机制
说到Handler就会被问到Handler、Message、Looper之间的关系了。那么Handler为什么要出现呢，它的作用是什么？

- Handler的作用
>Android是单线程模型的操作系统。为了避免多线程更新UI出现混乱，出现线程不安全，在Android中只能使用主线程（UI线程）更新UI，那么子线程和UI线程之间的通信怎么通信呢？Handler就是为了解决这个问题的。

- Handler、Message、Looper之间的关系
>Handler：消息处理者负责发送消息和消息内容的处理。sendMessage和handleMessage方法
Message:消息对象，信息的携带者。
Looper:它是消息的载体，Looper.loop()是一个死循环，会不断的从消息队列中取出消息。如果有消息就会处理，否则会阻塞。
MessageQueue:用来存放Handler发送的消息的消息队列(双向链表结构)。
</br>
</br>
从源码的角度来说，我们通过Handler发送Message到MessageQueue，MessageQueue调用enqueueMessage方法向消息队列中插入一条消息。Looper会不停的轮询Message，它是一个阻塞式死循环，当发现有消息的时候，会调用dispatchMessage方法分发给Handler，Handler通过handlerMessage进行处理这些消息。

### Android中的动画有哪几类？各自的特点和区别是什么？
动画可以分为三类，帧动画、补间动画、属性动画。
+ 帧动画(Frame)
>将每一张静止的图片依次的显示出来,通过animation-list标签包裹

+ 补间动画(Tween)
>补间动画只在视图层实现了动画，并没有改变View的本质。分别有平移(Translate)、缩放(scale)、旋转(rotate)、透明度(alpha)这四种

+ 属性动画(Property)
>属性动画可以作用于任何对象，并且是通过改变对象的属性值而实现的一种动画

**属性动画中重要的方法及分类**
###### ValueAnimator
>ValueAnimator是这个属性动画机制中最核心的类。因为属性动画就是通过不断的改变对象的属性值来实现的动画效果，初始值和结束值之间的动画过渡就是由ValueAnimator这个来完成就算的。

使用ValueAnimator将一个对象从0平滑过渡到1，间隔周期300ms。
```JAVA
ValueAnimator animator = ValueAnimator.ofFloat(0f, 1f);
animator.setDuration(300);
animator.start();
```

###### ObjectAnimator
>ObjectAnimator是最常用的属性动画，它继承玉ValueAnimator

将一张图片水平向右移动100px，然后回到原位
```java
// 沿X轴向移动100px，然后向左移动回到原位
// 沿Y轴移动，ofFloat第二个参数传入translationY
ObjectAnimator animator = ObjectAnimator.ofFloat(image, "translationX", 0f, 100f, 0f);
// 动画执行时长2s，默认300ms
animator.setDuration(2000);
animator.start();。
```
属性动画总图片的移动实现了图片真正意义上的移动，改变位置后可以点击，与此相比，补间动画只是改变了图片的显示效果，这也是它的缺点。

ofFloat方法中第二个参数可以传入任意对象的属性名，ObjectAnimator内部会查找该对象对应的get、set方法来设置属性值。

###### 组合动画
>组合动画需要借助AnimatorSet完成。通过实例化AnimatorSet.Builder实例后进行配置属性

图片从屏幕左侧移动到右侧，再回到原位，同时透明度从0调节到1，然后垂直旋转360度
```java
ObjectAnimator trans = ObjectAnimator.ofFloat(image, "translationX", -100f, 100f, 0f);
ObjectAnimator alpha = ObjectAnimator.ofFloat(image, "alpha", 0f, 1f);
ObjectAnimator rotation = ObjectAnimator.ofFloat(image, "rotationY", 0f, 360f);
AnimatorSet animatorSet = new AnimatorSet();
animatorSet.play(trans).with(alpha).before(rotation);
animatorSet.setDuration(5000);
animatorSet.start();
```

### 什么情况导致内存泄漏
+ 单例造成内存泄漏
>由于单例模式的静态的特性使得它的生命周期和应用一样长。如果一个对象不在使用了，而单例对象还持有该对象的引用，就会使得该对象不能正常回收，从而导致内存泄漏。

+ 非静态内部类创建静态实例造成的内存泄漏
> 例如，有时候我们会在启动频繁的Acitivity中，为了避免创建相同的数据资源可能会出现如下写法

  ```JAVA
  public class MainActivity extends AppCompatActivity {
     private static TestResource mResource = null;
     @Override
    protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
         setContentView(R.layout.activity_main);
         if(mResource == null){
            mResource = new TestResource();
         }
        //...
     }

   class TestResource {
    //...
     }
  }
  ```
>这样在Acitivity内部创建了一个非静态内部类的单例，每次启动都会使用该单例的数据，虽然这样避免了资源的重复创建，但这种写法会造成内存泄漏，因为非静态内部类默认会持有外部类的引用
而这个非静态内部类又创建了一个静态的实例，改实例和应用生命周期一样长，这就导致了该静态实例一直会持有该Acitivity的引用，从而导致了Activity的资源不能正常被回收。

  解决方法：
  >将该内部类设置为静态内部类，或者将给内部类抽取出来封装成一个单例，如果要使用Context就使用Application的Context。

+ Hanlder造成内存泄漏

  从Android的角度
  >当Android应用程序启动时，该应用程序的主线程会自动创建一个Looper对象和与之关联的MessageQueue。当主线程中实例化一个Handler对象后，它就会自动与主线程Looper的MessageQueue关联起来。所有发送到MessageQueue的Message都会持有Handler的引用，所以Looper会据此回调Handle的handleMessage()方法来处理消息。只要MessageQueue中有未处理的Message，Looper就会不断的从中取出并交给Handler处理。另外，主线程的Looper对象会伴随该应用程序的整个生命周期。

  Java角度
  >在Java中，非静态内部类和匿名类内部类都会潜在持有它们所属的外部类的引用，但是静态内部类却不会。

  >对上述的示例进行分析，当MainActivity结束时，未处理的消息持有handler的引用，而handler又持有它所属的外部类也就是MainActivity的引用。这条引用关系会一直保持直到消息得到处理，这样阻止了MainActivity被垃圾回收器回收，从而造成了内存泄漏。

  解决方法：将Handler类独立出来或者使用静态内部类，这样便可以避免内存泄漏。

+ 线程造成的内存泄漏
示例：AsyncTask和Runnable
>AsyncTask和Runnable都使用了匿名内部类，那么它们将持有其所在Activity的隐式引用。如果任务在Activity销毁之前还未完成，那么将导致Activity的内存资源无法被回收，从而造成内存泄漏。
解决方法：将AsyncTask和Runnable类独立出来或者使用静态内部类，这样便可以避免内存泄漏。

+ 资源未关闭造成的内存泄漏
>对于使用了BraodcastReceiver，ContentObserver，File，Cursor，Stream，Bitmap等资源，应该在Activity销毁时及时关闭或者注销，否则这些资源将不会被回收，从而造成内存泄漏。

  + 1）比如在Activity中register了一个BraodcastReceiver，但在Activity结束后没有unregister该BraodcastReceiver。

  + 2）资源性对象比如Cursor，Stream、File文件等往往都用了一些缓冲，我们在不使用的时候，应该及时关闭它们，以便它们的缓冲及时回收内存。它们的缓冲不仅存在于 java虚拟机内，还存在于java虚拟机外。如果我们仅仅是把它的引用设置为null，而不关闭它们，往往会造成内存泄漏。

  + 3）对于资源性对象在不使用的时候，应该调用它的close()函数将其关闭掉，然后再设置为null。在我们的程序退出时一定要确保我们的资源性对象已经关闭。

  + 4）Bitmap对象不在使用时调用recycle()释放内存。2.3以后的bitmap应该是不需要手动recycle了，内存已经在java层了。

+ 使用ListView时造成的内存泄漏
>初始时ListView会从BaseAdapter中根据当前的屏幕布局实例化一定数量的View对象，同时ListView会将这些View对象缓存起来。当向上滚动ListView时，原先位于最上面的Item的View对象会被回收，然后被用来构造新出现在下面的Item。这个构造过程就是由getView()方法完成的，getView()的第二个形参convertView就是被缓存起来的Item的View对象（初始化时缓存中没有View对象则convertView是null）。
构造Adapter时，没有使用缓存的convertView。

  解决方法：在构造Adapter时，使用缓存的convertView。

+ 集合容器中的内存泄露
  >我们通常把一些对象的引用加入到了集合容器（比如ArrayList）中，当我们不需要该对象时，并没有把它的引用从集合中清理掉，这样这个集合就会越来越大。如果这个集合是static的话，那情况就更严重了。

  解决方法：在退出程序之前，将集合里的东西clear，然后置为null，再退出程序。

+ WebView造成的泄露
>当我们不要使用WebView对象时，应该调用它的destory()函数来销毁它，并释放其占用的内存，否则其长期占用的内存也不能被回收，从而造成内存泄露。
  解决方法：
  为WebView另外开启一个进程，通过AIDL与主线程进行通信，WebView所在的进程可以根据业务的需要选择合适的时机进行销毁，从而达到内存的完整释放。
