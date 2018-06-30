# 生命周期

Libgdx 应用程序具有明确定义的生命周期，用于管理应用程序的状态。如创建，暂停，恢复，渲染和销毁等。

## ApplicationListener

开发人员实现 [ApplicationListener ](https://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/ApplicationListener.html)，然后将该类的实例传递给 `Application`，由 `Application`来处理（参考上一节 [基础框架介绍](/kai-fa-zhi-nan/ji-chu-kuang-jia.md)）。这样一来，每次发生程序级事件时，`Application`去调用实现类的对应方法。下面是一个简单的 `ApplicationListener`实现。

```java
public class MyGame implements ApplicationListener {
   public void create () {
   }

   public void render () {        
   }

   public void resize (int width, int height) { 
   }

   public void pause () { 
   }

   public void resume () {
   }

   public void dispose () { 
   }
}
```

另外，你也可以通过继承 [ApplicationAdapter](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/ApplicationAdapter.html) ，从而不去实现`ApplicationListener`的所有方法。

实例传递给 `Application` 之后，`ApplicationListener`调用方式如下：

| 方法名 | 描述 |
| :--- | :--- |
| create\(\) | 当 Application 创建时调用一次且只会调用一次。 |
| resize\(int width,int height\) | 这个方法将会在应用程序非暂停情况下，每次窗口大小改变时调起。另外，会在调起 create\(\) 方法之后调用一次。参数是每次窗口大小更新之后的宽高像素值。 |
| render\(\) | 应用程序每次执行循环都会调用该方法，通常也是在该方法内部执行游戏逻辑代码更新。 |
| pause\(\) | 在安卓平台上，按下 Home 键之后，会触发该方法。在桌面平台，只会在应用程序退出或关闭的时候，在 dispose\(\) 方法前面执行一次该方法。通常来说，这里是保存游戏状态的好地方。 |
| resume\(\) | 该方法只会在安卓平台触发，当游戏从暂停状态恢复时执行。 |
| dispose\(\) | 应用程序退出/关闭时执行，再执行该方法之前会先执行 pause\(\) 方法。 |

下图是能够直观说明生命周期：

![](/assets/1411img.png)

## 游戏的主循环在哪？

由于JavaScript 和 Android 的工作方式，Libgdx 以事件驱动为核心，没有显式主循环。但是， `ApplicationListener.render()` 可以视为主循环的主体。

## 还可以看看

如果你想要再安卓平台上处理动画逻辑，可以参考[这篇文章](http://bitiotic.com/blog/2013/05/23/libgdx-and-android-application-lifecycle/)。此文章还解释了为什么不推荐是用静态变量。

平台上

