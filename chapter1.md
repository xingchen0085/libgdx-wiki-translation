# Wiki 编辑指南

此页面会介绍编辑Wiki时需要注意的问题，给此Wiki贡献之前，请仔细阅读这部分内容。

## 问题

如果您对如何编辑此Wiki有任何问题，不要犹豫。[点击这里](https://github.com/libgdx/libgdx/wiki/Community-%26amp%3B-Support)访问我们的社区页面获取详情。

## 基础语法

此Wiki 使用的是 Markerdown 编写，[点击这里](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)获取 Markerdown 语法参考。请以这种方式编辑，防止出现差异。

## 风格

这一部分主要就是为了看上去风格基本一致。

确保每个标题前后有相同的符号出现，例如 `##test` 可以被解析为`<h2>` ，但是为了看起来更清晰，请使用 `##test##`代替 `##test` 。

当然，在每个标题后面，请留有空行。

#### 需要注意的语法

Wiki 的链接请使用`[[` 和 `]]` ，链接和文本之间请使用`|`分隔开来。

例子：

`[[link text to simple game | A simple game]]`将被解析为：[A simple game](https://github.com/libgdx/libgdx/wiki/A-simple-game)

注意不要使用常规的`[]()` 作为Wiki的链接。

#### 代码/文档链接

代码或者文档的链接，请以下述方法编写：`[ClassName](link to docs) [(code)](link to code)`，例子：

```
[Texture](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html)
[(code)](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/graphics/Texture.java)
```

将被渲染为：[Texture](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html) [\(code\)](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/graphics/Texture.java)

不要使用非字母字符作为 Wiki 页面名称，因为当Wiki 作为 Git 仓库导出后，不是所有的操作系统都能够处理或者支持这些字符。（比如 Windows 不支持 `" : "`）。

#### 文档链接注意

* 请注意 `ClassName (Code)` ， `Classname` 和 `(Code)` 之间有一个空格，以作区分。

* 请使用英文单词来格式化 `Classname (Code)` ，不要使用源代码或者其他派生词。

* 如果链接或者文档名称是以 `" ) "` 结尾的，Markerdown 会解析该 `" ) "`，导致链接混乱。以此为例：

```
http://libgdx.badlogicgames.comightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth()
```

上面的链接，经过Markerdown 解析之后就会混乱，所以需要记住 右括号`" ) "` 需要转义，如下：

```
[Link to Texture#getWidth](http:
//libgdx.badlogicgames.comightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth(\))
```

如果没有转义，就会出现 404 错误。

#### 视频

Github 不支持视频。所以我们可以使用这样的解决办法，给视频截一个图片，再将其链接到视频地址（youtube等）。下面是编辑语法：

```
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE" target="_blank">
 <img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg"
  alt="IMAGE ALT TEXT HERE" width="480" height="360" border="10" />
</a>
```

但愿 Github 能够尽快支持视频。

#### 边栏问题

在这个 Wiki 中使用边栏可能会导致原先的文本产生一些问题，































