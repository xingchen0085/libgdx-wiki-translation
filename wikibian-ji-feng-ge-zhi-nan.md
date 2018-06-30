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

* `Classname (Code)`  请使用常见英文单词，尽量不要使用源代码或者其他派生词。

* 如果链接或者文档名称是以 `" ) "` 结尾的，Markerdown 会解析该 `" ) "`，导致链接混乱。以此为例：

```
http://libgdx.badlogicgames.comightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth()
```

上面的链接，经过Markerdown 解析之后就会混乱，所以需要记住右括号`" ) "` 加上转义，如下：

```
[Link to Texture#getWidth](http:
//libgdx.badlogicgames.comightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth(\))
```

如果没有转义，就会出现 404 错误。

#### 视频

Github 不支持视频。所以我们可以使用这样的解决办法，给视频截取一个图片，再将其链接到视频地址（youtube等）。下面是编辑语法：

```
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE" target="_blank">
 <img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg"
  alt="IMAGE ALT TEXT HERE" width="480" height="360" border="10" />
</a>
```

但愿 Github 能够尽快支持视频。

#### 侧边栏问题

在这个 Wiki 中使用边栏可能会让原本的内容产生一些问题，比如下面这个例子：

`http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth()`

上面的链接添加之后可能会导致侧边栏的样式发生变化，为了解决此问题，可以把这样的长链接地址使用 三个反引号（\`\`\`）包围，并将其放置到下一行。参照 “**文档链接注意”** 部分检查链接地址。

#### 主目录（侧边栏展示）

如果您编写一个页面，您可能会希望将其显示到 Libgdx Wiki 目录和侧边栏目录中。为此，在创建您的章节时，在适当的位置创建一个二级目录，此时侧边栏的 Toc 目录列表将会看到变化，以保持一致。

##### 没有页面的目录

主目录中包含了少部分没有页面地址，并带有`??` 的标签，这是因为从 Google Code Wiki 翻译至 Github Wiki 时，这几个页面没有链接。如果关于这几个话题您有资料或者参考，随时欢迎添加你的页面和内容，并将链接更新到 Toc 目录。

#### 页面内的目录

每页的目录必须在页面内手动创建，关于本节之外的操作内容，可以参考 [box2d](https://github.com/libgdx/libgdx/wiki/Box2d) 这一章节（这一节中有页面内目录）。

我们使用Markerdown 编写标题时，会在标题名称前后加上不同数量的`#` ，来定义标题级别。当我们在一篇名为`Help me` 的章节中创建一个标题为 `## Comments and Questions/Concerns ##`  时，`help-me#comments-and-questionsconcerns` 就会链接到此内容（Help me 章节的地址为：`help-me`）。

所以我们这样编写标题之后，会生成一个无序列表形式的页面内容目录。可以前往 [box2d](#) 查看例子详情。

#### 添加图片

图片需要通过桌面形式手动添加\(不通过Github网页\)，图片都保存在本仓库的中的`images/` 文件夹内，添加图片需要将仓库 clone 出来：`$ git clone https://github.com/libgdx/libgdx.wiki.git` ，然后将您的图片添加到图片文件夹，使用适当的方式命名 `my-page-name#`，其中 \# 号是页面上显示图片的顺序（如果页面中只有一张图片，则可以省略。但不建议这样做）。

使用以下语法编写，即可显示图片（假设您的图片已经在 `images/`内）。

`[[images/using-libgdx-with-intellij-idea01.png]]`

括号之间没有空格。将会显示：

![](/assets/import.png)

#### 大型分页内容编辑

Github 的 Web 界面可以实现分页型的页面编辑Wiki，不过还是推荐采取以下步骤：

* Fork Libgdx Wiki 仓库
* 将您 Fork 下来的仓库 Clone 到本地
* 进行必要修改
* 提交和 Push 到您的仓库
* 给 Libgdx Wiki 的维护者（拥有提交权限）发送邮件，让他们 Clone 您的仓库并将其提交完善。

Github 没有针对 Wiki 有更强大的提交系统，所以Wiki修改只能以这种比较繁琐的方式进行，如果您有任何问题，可以联系 [support@github.com](mailto:support@github.com) 并附上您的建议（比如：希望 Github 的Wiki 能够更好! $IDEAS"）。如果您有问题，有必要向 Github 表达自己的想法。

#### 错别字修复

Github Wiki 的 DIFF 系统（文本比较）没有 代码的 DIFF 系统那样强大，如果您做了微小改变，请在提交时附上信息`Fixed typo alpa ->alpha` 或者其他类似的东西加以说明。

