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

### Fragment

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

**AndroidManifest.xml 文件**

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

**targetSdkVersion**

将其设置为你的安卓SDK版本。

**screenOrientation & configChanges**

除了设置SDK版本之外，activity 节点内的 `screenOrientation` 和 `configChanges `属性通常都需要配置。

`screenOrientation `这一属性指定了应用程序屏幕固定方向（横向还是纵向），如果应用程序同时支持横向和纵向，则可以忽略。

`configChanges`  属性很重要，而且应该配置上面的那些值。如果忽略该属性，意味着每次键盘的滑入/滑出和设备方向改变时，都会重新启动应用。如果`screenOrientation `不进行配置，Libgdx 在设备方向发生变化之后，会调用 `ApplicationListener.resize()` ，然后在API客户端进行相应的布局重排。

**Permissions**

如果应用程序需要使用设备的外部存储（比如SDcard），或者使用网络、手机振动、播放音乐等，需要在 `AndroidManifest.xml` 文件加上对应权限：

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/><!-- 存储 -->
<uses-permission android:name="android.permission.VIBRATE"/><!-- 振动 -->
<uses-permission android:name="android.permission.RECORD_AUDIO"/><!-- 音乐 -->
```

用户通常会不信任权限很多的应用，所以需要明智考虑需要使用那些。

需要保持屏幕常亮，需要将 `AndroidApplicationConfiguration.useWakeLock` 设置为 true 。

如果游戏中不需要加速度和指南针，建议将其设置为不可用。把AndroidApplicationConfiguration中的`useAccelerometer` 和 `useCompass`  设置为 false 。

如果你的游戏需要陀螺仪，可以把AndroidApplicationConfiguration的 `useGyroscope`  设置为true (这个值为了节省电量，默认是关闭的)。

你也可以[点击这里](https://developer.android.com/guide/)获取配置该文件的更多信息，比如设置应用的图标等。

### Live Wallpapers

Libgdx给安卓项目提供了以个简便的方法来作为入口类，就是 [Live Wallpapers](http://android-developers.blogspot.co.at/2010/02/live-wallpapers.html) 。这个类叫做 `AndroidLiveWallpaperService` ，下面是一个例子：

```java
package com.mypackage;

// 为了简洁省略导包的代码 

public class LiveWallpaper extends AndroidLiveWallpaperService {
	@Override
	public ApplicationListener createListener () {
		return new MyApplicationListener();
	}

	@Override
	public AndroidApplicationConfiguration createConfig () {
		return new AndroidApplicationConfiguration();
	}

	@Override
	public void offsetChange (ApplicationListener listener, float xOffset, float yOffset, float xOffsetStep, float yOffsetStep,
		int xPixelOffset, int yPixelOffset) {
		Gdx.app.log("LiveWallpaper", "offset changed: " + xOffset + ", " + yOffset);
	}
}
```

LiveWallpaper 被选取或者创建在屏幕显示时将会调用 `createListener()` 和 `createConfig()` 方法。

当用户在主屏幕上滑动时，会调用 `offsetChange()` 方法，会告诉你偏离中心的程度。这个方法将会在渲染的线程中调用，所以你不用同步任何东西。

添加LiveWallpaper 之后，你还需要创建一个XML文件来描述你的LiveWallpaper ，我们把它定义为  livewallpaper.xml ，在你的安卓项目的 `res/` 目录下创建 `xml/`目录，然后将这个xml文件放进去（res/xml/livewallpaper.xml ）。 下面是这个文件的内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper
       xmlns:android="http://schemas.android.com/apk/res/android"  
       android:thumbnail="@drawable/ic_launcher" 
       android:description="@string/description"
       android:settingsActivity="com.mypackage.LivewallpaperSettings"/>
```

这里定义了应用的图标，描述和用户点击"设置"时用作处理逻辑的一个Activity（LivewallpaperSettings）。LiveWallpaper提供了一个默认的Activity，里面包含了调节背景颜色和一些类似的基础功能部件。你可以保存这些设置到SharedPreferences 中，在ApplicationListener 中通过 `Gdx.app.getPreferences()` 加载这些设置。

最后，你需要在 `AndroidManifest.xml` 文件中加一些东西。下面是一个配置的例子：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.mypackage"
      android:versionCode="1"
      android:versionName="1.0"
      android:installLocation="preferExternal">
	<uses-sdk android:minSdkVersion="7" android:targetSdkVersion="14"/>	
	<uses-feature android:name="android.software.live_wallpaper" />
		
	<application android:icon="@drawable/icon" android:label="@string/app_name">
		<activity android:name=".LivewallpaperSettings" 
				  android:label="Livewallpaper Settings"/>
		
		<service android:name=".LiveWallpaper"
            android:label="@string/app_name"
            android:icon="@drawable/icon"
            android:permission="android.permission.BIND_WALLPAPER">
            <intent-filter>
                <action android:name="android.service.wallpaper.WallpaperService" />
            </intent-filter>
            <meta-data android:name="android.service.wallpaper"
                android:resource="@xml/livewallpaper" />
        </service>				  	
	</application>
</manifest> 
```

清单（文件）定义内容：

1. 使用LiveWallpaper功能，看这里：`<uses-feature/>`

2. 一个允许绑定LiveWallpaper的权限，看这里：`android:permission `

3. 注册“设置”的Activity

   `<activity android:name=".LivewallpaperSettings" android:label="LivewallpaperSettings"/>`

4. 注册LiveWallpaper的服务，指引。看这里：`<meta-data/>`

注意：LiveWallpaper仅支持Android 2.1 (SDK 7)以上版本。

LiveWallpaper通常在输入的时候会有一些限制，只会对点击/拖放产生反馈。如果你需要更多的多点触控事件，可以将`AndroidApplicationConfiguration#getTouchEventsForLiveWallpaper` 标记为true。

### Daydreams

在安卓4.2版本之后，可以给处于空闲的设备设置一些[屏保](https://developer.android.com/about/versions/android-4.2#Daydream)，比如显示图片等等。Libgdx可以帮你轻松实现这个功能。

屏保的入口类叫做AndroidDaydream ，下面是一个例子：

```java
package com.badlogic.gdx.tests.android;

import android.annotation.TargetApi;
import android.util.Log;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.backends.android.AndroidApplicationConfiguration;
import com.badlogic.gdx.backends.android.AndroidDaydream;
import com.badlogic.gdx.tests.MeshShaderTest;

@TargetApi(17)
public class Daydream extends AndroidDaydream {
   @Override
   public void onAttachedToWindow() {
      super.onAttachedToWindow();      
	  setInteractive(false);

      AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();
      ApplicationListener app = new MeshShaderTest();
      initialize(app, cfg);
   }
}
```

只需要继承AndroidDaydream，重写onAttachedToWindow()方法，然后配置AndroidApplicationConfiguration和ApplicationListener，再进行初始化。

除了Daydream本身，你也可以给Daydream提供设置，让用户配置Daydream。可以使一个普通的Activity，也可以是Libgdx的`AndroidApplication` 。下面是一个空Activity的例子：

```java
package com.badlogic.gdx.tests.android;

import android.app.Activity;

public class DaydreamSettings extends Activity {

}
```

必须将上述的设置Activity配置为Daydream 服务的元数据，在安卓项目的 `res/xml` 文件夹下创建一个xml，配置Activity。跟下面示例差不多：

```xml
<dream xmlns:android="http://schemas.android.com/apk/res/android"
 android:settingsActivity="com.badlogic.gdx.tests.android/.DaydreamSettings" />
```

最后，跟之前一样，在 AndroidManifest.xml 文件中添加Activity和Daydream 描述信息：

```xml
<service android:name=".Daydream"
   android:label="@string/app_name"
   android:icon="@drawable/icon"
   android:exported="true">
   <intent-filter>
	   <action android:name="android.service.dreams.DreamService" />
	   <category android:name="android.intent.category.DEFAULT" />
   </intent-filter>
   <meta-data android:name="android.service.dream"
	   android:resource="@xml/daydream" />
</service>
```

## iOS/Robovm

即将到来...

## HTML5/GWT

HTML5/GWT 应用主要入口是`GwtApplication` ，打开 `my-gdx-game-html5 `项目的 `GwtLauncher.java `类：

```java
package com.me.mygdxgame.client;

import com.me.mygdxgame.MyGdxGame;
import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.backends.gwt.GwtApplication;
import com.badlogic.gdx.backends.gwt.GwtApplicationConfiguration;

public class GwtLauncher extends GwtApplication {
   @Override
   public GwtApplicationConfiguration getConfig () {
      GwtApplicationConfiguration cfg = new GwtApplicationConfiguration(480, 320);
      return cfg;
   }

   @Override
   public ApplicationListener createApplicationListener () {
      return new MyGdxGame();
   }
}
```

起始类由 `GwtApplication.getConfig()` 和 `GwtApplication.createApplicationListener()` 两个方法组成，前者需要返回一个 [GwtApplicationConfiguration](https://github.com/libgdx/libgdx/blob/master/backends/gdx-backends-gwt/src/com/badlogic/gdx/backends/gwt/GwtApplicationConfiguration.java) 实例，该实例指定了HTML5应用程序的各种设置配置。`GwtApplication.createApplicatonListener()`  方法返回 `ApplicationListener` 用来运行。





























