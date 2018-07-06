# 起始类和配置

对于每个平台，都需要写一个起始类（入口类）。将会在这个类中实例化特定的实现了 `ApplicationListener`的一个 `Application`对象,整个游戏逻辑是在这个 `Application`类中编写。起始类是依赖于平台的，让我们看看每个平台如何实例化和配置。

阅读这篇文章的前提是你已经配置好项目环境（配置[参考这里](https://github.com/libgdx/libgdx/wiki/Project-setup,-running-&-debugging)），已经编写 core，desktop，Android 和HTML 这5个子项目的代码。

## 桌面（LWJGL）

桌面环境对应的就是 desktop 子项目。

打开 `my-gdx-game` 的 `Main.java` 类，将会看到下面的代码：

```java
package com.me.mygdxgame;

import com.badlogic.gdx.backends.lwjgl.LwjglApplication;
import com.badlogic.gdx.backends.lwjgl.LwjglApplicationConfiguration;

public class Main {
   public static void main(String[] args) {
      LwjglApplicationConfiguration cfg = new LwjglApplicationConfiguration();
      cfg.title = "my-gdx-game";
      cfg.useGL30 = false;
      cfg.width = 480;
      cfg.height = 320;

      new LwjglApplication(new MyGdxGame(), cfg);
   }
}
```

首先实例化 [LwjglApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-lwjgl/src/com/badlogic/gdx/backends/lwjgl/LwjglApplicationConfiguration.java) 对象，这个对象可以指定一些特定配置，比如初始窗口的信息，用 OpenGL 2.0 还是 3.0（试验版本）等等。前往 [JavaDoc ](https://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/backends/lwjgl/LwjglApplicationConfiguration.html)查看这个类的更多信息。

配置好片配置类后，就开始实例化 `LwjglApplication`，参数中的 `MyGdxGame()` 是实现了 `Application`接口，处理游戏主要逻辑的一个类。

之后，将会创建一个游戏窗口，根据[生命周期](/kai-fa-zhi-nan/ji-chu-kuang-jia/sheng-ming-zhou-qi.md) ， `ApplicationListener`进行相应的调用和处理。（正式进入游戏）

## 桌面（LWJGL3）

即将到来...

## 安卓

安卓项目没有`Main`方法作为入口点，不过可以使用`Activity`替代。打开 子项目 `my-gdx-game-android` 的 `MainActivity.java` 类。

```java
package com.me.mygdxgame;

import android.os.Bundle;

import com.badlogic.gdx.backends.android.AndroidApplication;
import com.badlogic.gdx.backends.android.AndroidApplicationConfiguration;

public class MainActivity extends AndroidApplication {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();

        initialize(new MyGdxGame(), cfg);
    }
}
```

使用`Activity` 的 `onCreate()`方法作为入口方法。需要注意的是，`MainActivity`继承了 `AndroidApplication`，另外 `AndroidApplication`本身也继承了 `Activity`.跟桌面环境差不多，也要创建一个配置类（[AndroidApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-android/src/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.java)）。配置完成之后，调用 `AndroidApplication.initialize()`方法，将 `ApplicationListener` 实现实例`MyGdxGame()`传递给此方法。关于[AndroidApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-android/src/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.java) 的更多信息，可以[点击JavaDoc](https://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.html) 查看可以配置那些选项。

安卓应用通常会有多个 Activity ，但 Libgds 项目通常只会存在一个 Activity ，在Libgdx 中，可以使用多个不同的 Screen 来切换不同场景，而不是使用多个 Activity。原因是每创建一个 Activity ，都会创建一个新的 OpenGL 上下文，这是一个耗时的操作，而且还会重新加载游戏资源。

### 基于Fragment 的 Libgdx

安卓的SDK 介绍了一种 API ，可以在界面上创建一块可控区域，它用来做不同的 Screen 在合适不过了。这个 API 就是 [Fragments API](http://developer.android.com/guide/components/fragments.html) 。Libgdx 现在也可以用作是 Fragment 中的一块很大的区域。创建一个 Libgdx 区域，可以像下面示例一样，继承 `AndroidFragmentApplication`之后，重写 `onCreateView()`方法：

```java
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    return initializeForView(new MyGdxGame());
}
```

这样一来，安卓子项目`my-gdx-game-android`将要作以下变化：

1. 如果没有添加 [Android V4 ](https://developer.android.com/tools/support-library/setup.html#libs-without-res)支持库，需要添加（点击获取帮助）。因为我们需要继承 `FragmentActivity` 这个类。
2. 应用程序入口 Activity 将原本继承的 AndroidApplication 改为 FragmentActivity。
3. 在应用程序入口 Activity 中实现 AndroidFragmentApplication 的回调函数。
4. 创建一个静态内部类来继承 AndroidFragmentApplication 以供 Libgdx 使用。
5. 在 onCreateView 方法中调用 initializeForView\(\) 方法。
6. 最后，将原本安卓启动类的Activity 替换为 Libgdx 的 Fragment。

例如：

```java
// 2. 应用程序入口 Activity 将原本继承的 AndroidApplication 改为 FragmentActivity
// 3. 在应用程序入口 Activity 中实现 AndroidFragmentApplication 的回调函数
public class AndroidLauncher extends FragmentActivity implements AndroidFragmentApplication.Callbacks
{
   @Override
   protected void onCreate (Bundle savedInstanceState)
   {
      super.onCreate(savedInstanceState);

      // 6. 最后，将原本安卓启动类的Activity 替换为 Libgdx 的 Fragment.
      GameFragment fragment = new GameFragment();
      FragmentTransaction trans = getSupportFragmentManager().beginTransaction();
      trans.replace(android.R.id.content, fragment);
      trans.commit();
   }

   // 4.创建一个静态内部类来继承 AndroidFragmentApplication 以供 Libgdx 使用.
   public static class GameFragment extends AndroidFragmentApplication
   {
      // 5.在 onCreateView 方法中调用 initializeForView() 方法.
      @Override
      public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
      {  return initializeForView(new MyGdxGame());   }
   }


   @Override
   public void exit() {}
}
```

### AndroidManifest.xml 文件

除了 `AndroidApplicationConfiguration` 之外，对于安卓项目，我们还需要配置 `AndroidManifest.xml` 文件。你可以在安卓项目的根目录找到它，看起来大概是下面这个样子：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.me.mygdxgame"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="8" android:targetSdkVersion="15" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:screenOrientation="landscape"
            android:configChanges="keyboard|keyboardHidden|orientation">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

#### targetSdkVersion

将其设置为你的安卓SDK版本。

#### screenOrientation & configChanges

除了设置SDK版本之外，activity 节点内的 `screenOrientation` 和 `configChanges `属性通常都需要配置。

`screenOrientation `这一属性指定了应用程序屏幕固定方向（横向还是纵向），如果应用程序同时支持横向和纵向，则可以忽略。





























