# 组件概述

## 介绍

Libgdx 包含了典型游戏架构的每个步骤提供服务的所有组件。

* [_**Input**_](https://github.com/libgdx/libgdx/wiki/Input-handling) - 为所有平台提供统一的输入模型和处理程序。支持触摸屏，键盘，加速度计和鼠标。
* [_**Graphics**_](https://github.com/libgdx/libgdx/wiki/Graphics) - 由底层的 OpenGL 提供的实现，将纹理图片绘制到屏幕上。
* [_**Files**_](https://github.com/libgdx/libgdx/wiki/File-handling) - 无论是什么平台，都可以通过此组件方便的进行文件读写。
* [_**Audio**_](https://github.com/libgdx/libgdx/wiki/Audio) -跨平台实现录音和播放。
* [_**Networking**_](https://github.com/libgdx/libgdx/wiki/Networking) - 提供执行网络的方法。比如简单的HTTP GET/POST 请求，包括 TCP服务端/客户端 Socket链接。

下面的示意图讲展示简单的游戏架构中的组件关系。

![](/assets/zjgs1.png)

下面将会对每个组件进行简单的介绍和提供常见用例。

## 组件

### Input 

_Input _实现了各个平台的输入状态轮询监听，可以轮询每个按键、触屏和加速度。不过在桌面环境下，触屏将被替换为鼠标点击，加速度也无法使用。

它还提供了注册输入事件，用来处理基于事件的输入模型的方法。

下面的代码片段是为了在游戏运行过程中，获取触摸点的坐标（如果在桌面环境，就是获取鼠标点击点的坐标）。

```java
if (Gdx.input.isTouched()) {
  System.out.println("Input occurred at x=" + Gdx.input.getX() + ", y=" + Gdx.input.getY()); 
}
```

类似的方式，可以轮询和处理所有支持的输入方式。

### Graphics

_Graphics _抽象了与GPU的通信，也可以通过此模块获取OpenGL ES 包装器的实例。该组件实现了OpenGL所提供的所有接口和方法。

由于底层不同，组件可能也会出现不可用。

_Graphics _提供了生成位图和纹理的方法。

比如，需要获取 OpenGL API 2.0 对象实例，可以使用下面的代码：

```java
GL20 gl = Gdx.graphics.getGL20 ();
```

该方法会返回一个可以用来绘制屏幕内容的实例对象，如果硬件环境不支持OpenGL2.0，将会返回null.

下面的代码片段作用是清屏并把屏幕绘制成红色。

```java
gl.glClearColor(1f, 0.0f, 0.0f, 1);
gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
```

它总会根据环境（lwjgl ，jogl 或 android）返回一个处理实例，因此，主程序不需要知道处理细节，只要支持，在不同平台都能正常工作。

下面是支持的API版本：

| GL 版本 | 访问的方法 |
| :--- | :--- |
| 2.0 | Gdx.graphics.getGL20\(\); |
| 3.0 | Gdx.graphics.getGL30\(\); |

要了解更多关于 _Graphics  _组件的内容，可以[点击这里](https://github.com/libgdx/libgdx/wiki/Graphics-module)查看文档。

### Files

_Files _组件提供了一个通用的方法来访问不同环境下的文件系统，让读写文件更加简便。不过在写文件上，可能不同平台会有一定限制。

该组件，最常用的就是用来从同一个二级目录中加载游戏资源文件（纹理，声音文件等）供各个平台使用。比如将游戏得分和游戏状态写入文件也是非常有用。

下面的例子，会从 $APP\_DIR/assets/textures 目录中加载一张图片。

```java
Texture myTexture = new Texture(Gdx.files.internal("assets/textures/brick.png"));
```

这是一个非常强大文件加载方式，因为它适用于Android和桌面环境。

### Audio

_Audio _组件让创建和播放音频变得非常方便，也提供了访问物理环境音频的方法。

它可以处理两种声音，音效（Sound）和音乐（Music），这两种类型声音都支持 WAV，Mp3 和 OGG 格式。

音效（Sound）对象将会被加载到内存中，可以在任何时间播放。非常适合多次使用的声音特效，比如爆炸和枪声。

音乐（Music）对象则是以另一种方式，从硬盘（或者是 SD card）中读取声音文件流。每次播放，都会以文件流形式传输到音频设备。

下面的代码片段展示了从硬盘中加载一个音频文件，以0.5倍音量和循环播放方式播放 _myMusicFile.mp3._

```java
Music music = Gdx.audio.newMusic(Gdx.files.getFileHandle("data/myMusicFile.mp3", FileType.Internal));
music.setVolume(0.5f);
music.play();
music.setLooping(true);
```

### Networking

_Networking_ 组件，在网络游戏中，添加多个玩家，把玩家信息传输到服务器，或者执行其他网络任务时，显得极其重要。这些功能可以每个平台使用，但是需要注意有的平台可能有一些注意事项或者缺少一些功能。

该模块包含了可配置的 TCP 客户端和服务端套接字，为了解决延迟问题，做了配置优化。

也包含了发起HTTP请求的方法和实用程序，要实现这样的需求，可以使用 Request Builder.

下面的代码片段可以通过 Request Builder 发起一个 HTTP 请求。

```java
HttpRequestBuilder requestBuilder = new HttpRequestBuilder();
HttpRequest httpRequest = requestBuilder.newRequest()
   .method(HttpMethods.GET)
   .url("http://www.google.de")
   .build();
Gdx.net.sendHttpRequest(httpRequest, httpResponseListener);
```

也可以使用以下方法发起带参数的 HTTP 请求。

```java
HttpRequestBuilder requestBuilder = new HttpRequestBuilder();
HttpRequest httpRequest = requestBuilder.newRequest()
   .method(HttpMethods.GET)
   .url("http://www.google.de")
   .content("q=libgdx&example=example")
   .build();
Gdx.net.sendHttpRequest(httpRequest, httpResponseListener);
```



























