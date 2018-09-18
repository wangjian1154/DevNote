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
>属性动画可以作用于任何对象
