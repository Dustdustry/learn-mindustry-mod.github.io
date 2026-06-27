# 图形化页面 UI

UI 用于向玩家展示信息（数据库、小地图等），提供交互入口（选择建造的建筑、修改建筑配置、蓝图管理等）

<ImageGrid cols="2" gap=40>
  <ImageItem height=200 src="./imgs/database.png" caption="展示信息" />
  <ImageItem height=200 src="./imgs/config.png" caption="提供交互入口" />
</ImageGrid>

# Hello World - 入门 UI

我们暂时先不追究原理，先在 `main.js` 中手动敲入以下代码：

```js
Events.on(ClientLoadEvent, (e) => {
  const dialog = new BaseDialog("Learn Mindustry UI")
  dialog.cont.add("Hello World!")
  dialog.addCloseButton()
  dialog.show()
})
```

保存后打开游戏，运行正常时，你会看到如下页面：

<ImageItem src="./imgs/hello-world.png"></ImageItem>

这样，我们就写出了第一份页面，之后的教程也将以此页面为基础，一步一步的引入更多UI的内容。

# My First Page - 初识页面

我们先修改 BaseDialog 的参数，看看页面会有什么不同：

```js
Events.on(ClientLoadEvent, (e) => {
  const dialog = new BaseDialog("Learn Mindustry UI") // [!code --]
  const dialog = new BaseDialog("My First Page") // [!code ++]
  dialog.cont.add("Hello World!")
  dialog.addCloseButton()
  dialog.show()
})
```

进入游戏后注意观察页面顶部，可以发现，页面顶部的标题发生了变动。

没错，BaseDialog 的第一个参数正是页面的**标题(title)**。

<ImageGrid cols="2" gap=40>
    <ImageItem width=200 src="./imgs/title-before.png" caption="修改前" />
    <ImageItem width=200 src="./imgs/title-after.png" caption="修改后" />
</ImageGrid>

---

同样，你也可以试试修改 `"Hello World!"` 看看页面会发生什么变化：

```js
Events.on(ClientLoadEvent, (e) => {
  const dialog = new BaseDialog("My First Page")
  dialog.cont.add("Hello World!") // [!code --]
  dialog.cont.add("Welcome to my first page!") // [!code ++]
  dialog.addCloseButton()
  dialog.show()
})
```

可以看到，页面中央的 **文本(Text)** 发生了变化：

<ImageGrid cols="2" gap=40>
    <ImageItem width=200 src="./imgs/cont-text-before.png" caption="修改前" />
    <ImageItem width=200 src="./imgs/cont-text-after.png" caption="修改后" />
</ImageGrid>

---

有了上述的探索，你应该能感受到下面部分代码的作用：

1. 我们是先创建了一个**标题**为 `My First Page` 的**页面**
2. 往页面上添加了**文本** `Welcome to my first page!`
3. 向页面添加了返回**按钮(Button)**
4. 把页面显示出来

```js
Events.on(ClientLoadEvent, (e) => {
  const dialog = new BaseDialog("My First Page") // 创建页面
  dialog.cont.add("Welcome to my first page!") // 添加文本
  dialog.addCloseButton() // 添加返回按钮
  dialog.show() // 显示页面
})
```

经过上述解释，现在你应该已经了解了一个页面的创建和显示过程。

现在你肯定还有一些疑惑：

1.  为什么我要写 `Events.on(ClientLoadEvent, (e) => {...})`，这是必须的吗？
    - 对，这是**必须**的。不过至于为什么，我们按住不表，后续会给出解释，我们先按照已给出的代码，一步一步学习。

---

# Label & Image - 添加文本、图片

现在，我们进一步添加新的内容：

```js
Events.on(ClientLoadEvent, (e) => {
  const dialog = new BaseDialog("My First Page")
  dialog.cont.add("Welcome to my first page!")
  dialog.cont.add("Mono is mining") // [!code ++]
  dialog.cont.image(Items.copper.uiIcon) // [!code ++]
  dialog.addCloseButton()
  dialog.show()
})
```

1. 调用 `add`，添加新文本
2. 调用 `image`，并传入图片，添加**图片(Image)**

这样，我们的页面就新增了文本和图片：
<ImageItem src="./imgs/text-image.png" caption="新增文本和图片" />

图片的来源非常广泛，在我们的代码里，通过 `Items.copper.uiIcon` 获取到了铜的图标。

任何`UnlockableContent`都有`uiIcon`的图标字段，因此你也可以试着把铜改成单位、建筑、液体等，来获取它们的图标。

除了获取内容的图标，你还可以获取游戏内的其他图标，这些图标都被放在了编译时生成的类`Icon`中。你可以在 LMM 群文件找到 `Icon` 文件。

接下来我们就引入更多图片开源：

```js
Events.on(ClientLoadEvent, (e) => {
  const dialog = new BaseDialog("My First Page")
  dialog.cont.add("Welcome to my first page!")
  dialog.cont.add("Mono is mining")
  dialog.cont.image(Items.copper.uiIcon)

  dialog.cont.image(UnitTypes.flare.uiIcon) // [!code ++]
  dialog.cont.image(Blocks.duo.uiIcon) // [!code ++]
  dialog.cont.image(Liquids.water.uiIcon) // [!code ++]
  dialog.cont.image(Icon.upload) // [!code ++]
  dialog.cont.image(Icon.cancel) // [!code ++]

  dialog.addCloseButton()
  dialog.show()
})
```

<ImageItem src="./imgs/more-image.png" caption="新增更多图片!" />

# Table - 初识表格

> 施工中...

# Cell - 表格布局

> 施工中...
