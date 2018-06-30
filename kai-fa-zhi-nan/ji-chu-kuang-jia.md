# 基础框架

## 组件

Libgdx 的核心由以下 6 个接口组成，由这 6 个组件负责与操作系统交互。每个后端都是实现了自己的接口。

* Application ：运行应用程序并向API客户端发送应用程序事件，例如窗口大小调整等。还提供了日志记录和系统运行情况查询，比如内存使用情况。
* Files ：管理平台的底层文件系统，在不同文件系统之上（不与JAVA的File类互操作）建立各文件位置的抽象。
* Input ：处理API客户端的用户输入事件，比如：键盘，鼠标，触摸或加速度计事件。支持轮询和事件驱动的处理。
* Net ：提供跨平台式的 HTTP/HTTPS 访问资源的方法，以及创建TCP套接字的服务端和客户端。
* Audio ：提供播放音效和流式音乐的方法，还可以直接访问音频设备直接进行PCM 音频的输入和输出。
* Graphics ：实现与OpenGL ES 2.0\(如果可用\)底层接口的交互 ，提供动画/模型相关的查询和设置（计算机图形学）。

## 启动类

唯一需要根据不同平台编写不同的代码，就是启动类。对于不同的平台，有着不同的实例化实现。比如桌面端的启动类代码可能就跟以下示例类似，其底层实现是 Lwjgl。

```java
public class DesktopStarter {
   public static void main(String[] argv) {
      LwjglApplicationConfiguration config = new LwjglApplicationConfiguration();
      new LwjglApplication(new MyGame(), config);
   }
}
```

对于安卓端，启动类看起来是下面这样：

```java
public class AndroidStarter extends AndroidApplication {
   public void onCreate(Bundle bundle) {
      super.onCreate(bundle);
      AndroidApplicationConfiguration config = new AndroidApplicationConfiguration();
      initialize(new MyGame(), config);
   }
}
```



