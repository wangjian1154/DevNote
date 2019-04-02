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


### Activity A启动另一个Activity B会回调哪些方法？如果Activity B是完全透明呢？如果启动的是一个Dialog呢？
>Activity A启动另一个Activity B会回调的方法：Activity A的onPause()——>Activity B的onCreate()——>onStart()——>onResume()——>Activity A的onStop();如果Activity B是完全透明的，则最后不会调用Activity的onStop(),如果是Dialog，也不会

### 热修复原理
>热修复：让应用能够在无需重新安装的情况实现更新，帮助应用建立动态修复的能力

</br>
**热修复分为三大流派**
- Native方案：AndFix、KKFix等
- Java方案：Qzone的超级补丁、Tinker、Robust
- 混合方案：Sophix

**AndFix** 采用Native Hook的方式，这套方案直接使用dalvik_replaceMethod替换class中方法的实现，由于它并没有整体替换class，而field在class中的相对地址在class加载时已确定，所以AndFix无法支持新增或者删除filed的情况（通过替换init与clinit只可以修改field的数值）。
也正因为如此，AndFix可以支持的补丁场景相对有限，仅仅可以使用它来修复特点的问题。另外使用
Native替换将会面临比较复杂的兼容性的问题，最大的优点在于立即生效。
</br>
**Qzone** 方案并没有开源，但在github上Nuwa采用了相同的方式，这个方案使用classloader的方式，能够实现更加友好的类替换。而且这与我们加载Multidex的做法相似，能基本保证稳定性和兼容性。为了解决unexpected DEX problem异常而采用插桩的方式，从而规避了问题的出现，事实上，Android系统的这些检查规则是非常有意义的，这会导致Qzone方案在Dalvik与Art会产生一些问题。
</br>
**Tinker** 是全量替换新的Dex，为了减轻包的大小，微信自研了DexDiff算法，他深度利用Dex的
格式来减少差异的大小。简单来说，在编译时通过新旧两个Dex生成差异path.dex。在运行时，将差异patch加载。

### ANR的原因
- 耗时的网络访问
- 大量的数据读写
- 数据库操作
- 硬件操作（如Camera）
- 调用thread的join()方法、sleep()方法、wait()方法或者等待线程锁的时候
- service binder的数量达到上限
- system server中发生WatchDog ANR
- service忙导致超时无响应
- 其他线程持有锁，导致主线程等待超时
- 其它线程终止或崩溃导致主线程一直等待

### 图片的三级缓存原理
>当Android端需要获取数据时候，例如获取网络图片，首先从内存中根据键名查找，内存中没有就会去磁盘或者数据库中查找，若再没有会从网络获取，并存入内存和磁盘或数据库。

### LruCache底层原理
>LruCache中的Lru算法就是通过LinkedHashMap来实现的，LinkedHashMap继承于HashMap，它使用双向列表来存储Map中Entry顺序关系，对于get，put，remove等操作。LinkedHashMap
除了要做HashMap要做的事情，还做些调整Entry顺序链表的工作。LruCache中将LinkedHashMap的顺序设置为LRU顺序来实现LRU缓存，每次调用get(也就是从内存缓存中取图片)，则将该对象移到链表的尾端。
调用put插入新的对象也是存储在链表尾端，这样当内存缓存达到设定的最大值时，将链表头部的对象（近期最少用到的）移除。

### AIDL的使用与原理
>AIDL是Android Interface Definition Language的缩写，也就是Android接口定义语言，我们在写完AIDL文件后，系统会自动生成一个同名的.java文件，在服务端和客户端也可以照常使用
这个类进行跨进程通信。

### Activity如与Service通信
>可以使用bindService的方式，现在Acitivity中实现一个ServiceConnection接口，并且将该接口传递给bindService()方法，在ServiceConnect接口的onServiceConnectioned()方法里执行相关操作。

### 遇到过哪些关于Fragment的问题，如何处理的
- getActivity()空指针：这种情况一般发生在异步线程调用getActivity(),而fragment已经onDetach(),此时就会空指针，解决方法是在Fragment里使用一个全局变量mActivity，在onAttached里赋值，这样可能会
引起内存泄漏，但是异步线程没有停止的情况本身就可能导致内存泄漏，相比空指针，这种情况更佳。
- Fragment视图重叠：在onCreate方法加载Fragment，并没有判断saveInstanceState==null或if(findFragmentByTag(mFragmentTag) == null)，导致重复加载了一个Fragment导致重叠，解决方法判空。

### 描述下View的绘制原理
View的绘制主要分为三步
- onMeaSure：测了视图大小，从顶层父级View到子View递归调用measure()方法，measure()调用onMeasure()方法，onMeasure方法完成绘制工作，
- onLayout：确定视图位置，从顶层父View到子View递归调用layout()方法，父View将上一步measure()方法得到的子View的布局大小和布局参数，将子View放在合适的位置。
- onDraw：绘制最终视图，首先ViewRoot创建一个Canvas对象，然后调用onDraw()方法进行绘制，onDraw方法绘制顺序是：绘制视图背景，绘制画布的图层，绘制View的内容，绘制子视图如果有的话，还原图层，绘制滚动条。

### Serializable和Parcelable区别
>Serializable的作用是为了保存对象的属性到本地文件、数据库、网络流、RMI以方便数据传输，当然这种传输可以是程序内也可以是两个程序之间，使用了反射技术，并且期间产生临时对象。Android的Parcelable的设计初衷
是因为Serializable执行效率慢。为了在程序内各个组件和程序之间高效进行数据传输而设计。这些数据仅在内存中存在，Parcelable是通过IBinder通信的消息的载体。

Parcelable的性能比Serializable好，在内存开销方面较小，所以在内存间数据传输时推荐使用Parcelable，如activity间传输数据，而Serializable可将数据持久化方便保存，所以在需要保存或网络传输数据时选择Serializable，因为android不同版本Parcelable可能不同，所以不推荐使用Parcelable进行数据持久化
