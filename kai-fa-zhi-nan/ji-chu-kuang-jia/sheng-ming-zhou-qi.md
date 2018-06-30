# 生命周期

Libgdx 应用程序具有明确定义的生命周期，用于管理应用程序的状态。如创建，暂停，恢复，渲染和销毁等。

## ApplicationListener

开发人员实现 [ApplicationListener ](https://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/ApplicationListener.html)，然后将该类的实例传递给 `Application`，由 `Application `来处理（参考上一节 [基础框架介绍](/kai-fa-zhi-nan/ji-chu-kuang-jia.md)）。这样一来，每次发生程序级事件时，`Application `会触发`ApplicationListener `事件。下面是一个很简单的 `ApplicationListener `实现。

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

另外，你也可以通过继承 [ApplicationAdapter](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/ApplicationAdapter.html) ，从而不去实现`ApplicationListener `的所有方法。

一旦传递给 `Application `，`ApplicationListener `调用情况如下所示：

| 方法名 | 描述 |
| :--- | :--- |
| create\(\) | 当 Application 创建时调用一次且只会调用一次 |



