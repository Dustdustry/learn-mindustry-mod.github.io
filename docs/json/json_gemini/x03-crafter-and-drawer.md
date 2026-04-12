# 0x03 工厂、方块通性与绘制器

> ***“机械的轰鸣是工业的心跳。”***

现在我们已经拥有了基础的材料（如“硅钢”），接下来必须要有机器来生产和处理它们。在 Mindustry 中，方块（Block）是构成一切的物理实体。

本章，我们将解析方块的共有属性，学习如何创建一个基础加工厂，并深入探究 v7 时代强大的多层绘制器系统（Drawer System）。

**学习本章需要具备的基础：**
- 已经掌握了 0x02 中物品的创建与引用方式。

---

## 方块通性 (Block Basics)

无论是工厂、炮塔还是发电机，它们在底层都继承自同一个基础类，因此拥有一批共用的属性。
创建 `content/blocks/` 文件夹。以后所有的方块都会放在这里（或者它的子文件夹里，例如 `content/blocks/production/` 以方便管理）。

新建一个名为 `silicon-steel-smelter.hjson`（硅钢冶炼厂）的文件。我们先看它的共通属性：

```hjson
{
  // 所有的方块都必须声明它具体属于哪种子类型，GenericCrafter 是最基础的通用加工厂
  type: GenericCrafter
  
  // 占地大小：3 代表这是一个 3x3 的方块
  size: 3
  
  // 血量：方块承受伤害的上限
  health: 400
  
  // 建造需求：玩家需要花费什么材料才能建造它。还记得 Hjson 的特殊缩写吗？
  requirements: [
    copper/100
    lead/50
    graphite/20
  ]
  
  // 所属分类：决定了它在 UI 底部的哪个分类栏里出现（这里是 '制作' 分类）
  category: crafting
}
```

::: info 原版的分类 (Category)
常见的分类有：`crafting` (制作), `power` (电力), `turret` (炮塔), `defense` (防御墙), `distribution` (物流物流), `liquid` (流体), `effect` (效果/存储)。
:::

---

## 消耗器 (Consumers) 与加工逻辑

一个工厂没有原料摄入是无法工作的。在 Mindustry 中，我们使用 `consumes`（消耗）来定义它运作所需的代价。
接着在上面的文件里添加：

```hjson
  // 加工时间：制作一次产物需要花费的 Tick 数（游戏固定每秒 60 Tick，因此 60 代表 1 秒）
  craftTime: 90
  
  // 最终产物：支持 "物品名/数量" 缩写
  // 注意，这里的 silicon-steel 需要加上你的模组前缀！
  outputItem: my-first-mod-silicon-steel/2
  
  // 消耗器
  consumes: {
    // 消耗的物品清单
    items: [
      silicon/2
      titanium/1
    ]
    
    // 运行时的电力消耗，数值为 每Tick消耗量。
    // 如果你想让它每秒消耗 60 电力，这里就填 1（1 * 60 = 60）
    power: 1.5
    
    // （可选）如果需要消耗流体，例如水
    // liquids: [ water/0.2 ]
  }
```

::: tip 物品容量
默认情况下，工厂的每种物品的内部容量是 `24`。如果你的消耗或产出数量极大，你必须手动提升 `itemCapacity`，否则工厂会因为塞不进原料而罢工！
:::

---

## 绘制器 (Drawers)：让方块“活”起来

在旧版本中，让工厂运转时发光、冒火是一件非常折磨的事情。而在 v7 时代，游戏引入了高度自由的 **多层绘制器 (DrawMulti)** 系统。《饱和火力》中的很多大型高炉之所以看起来视觉效果拉满，正是因为组合使用了它。

我们为“硅钢冶炼厂”加上炫酷的绘制效果：

```hjson
  drawer: {
    // 声明这是一个多层绘制器
    type: DrawMulti
    
    // 按照从下到上（从背景到前景）的顺序叠放图层
    drawers: [
      // 第一层：绘制方块的基础默认贴图 (也就是你的 silicon-steel-smelter.png)
      { type: DrawDefault }
      
      // 第二层：绘制内部流动的发光液体效果（它会自动读取方块当前内部的流体颜色）
      // { type: DrawLiquidRegion, drawLiquid: water }
      
      // 第三层：绘制高炉运转时的耀眼火光
      {
        type: DrawFlame
        flameColor: a8baf2A8 // 火焰的颜色，带透明度
        flameRadius: 2.5     // 火焰的基础半径
        flameRadiusMag: 1    // 随运转强度的变化幅度
      }
      
      // 第四层：绘制发光晕影，让高炉周围产生光晕
      {
        type: DrawGlowRegion
        glowScale: 8
        alpha: 0.8
      }
    ]
  }
```

::: warning 贴图要求
如果你使用了特定的绘制器图层（例如 `DrawRegion` 或需要顶盖覆盖火焰的逻辑），你可能需要在 `sprites/` 目录下准备多张贴图。比如 `silicon-steel-smelter-bottom.png` 或 `silicon-steel-smelter-top.png`。详细的后缀要求建议随时参考原版源码中对应方块的写法！
:::

至此，一个从原料到产出，拥有光影特效的标准加工厂就诞生了！在下一章，我们将进一步探索更高级的生产机制以及支撑这一切的电力网络。