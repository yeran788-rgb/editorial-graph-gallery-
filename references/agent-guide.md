# Agent 实战指南 — 从画廊到数据页

Agent 用真实数据产出图表页的标准流程。核心思想：**画廊本身就是模板源，拆页组装，不改画廊原文件**。

## 1. 拆页公式

目标：把 N 个图型的数据版组装成独立单文件 HTML。

```
新页面 = 画廊前 ~170 行框架          ← <!DOCTYPE> → CSS → THEMES → 工具函数（到 fig 定义之前）
        + N 个数据版 fig({...})      ← 每个图型一个块，数据写进 draw 内
        + 画廊尾段                    ← 从「挂载·交互·重播」注释起：渲染循环、tooltip、REPLAY、主题按钮
```

操作步骤：

1. 打开 `assets/editorial-graph-gallery.html`，定位 `<script>` 起始到第一个 `fig({` 之前的框架段（CSS 变量、THEMES、`inkA()/accA()`、fig() 定义、渲染器）——原样复制。
2. 定位尾段挂载区（搜索 `挂载` 或 `renderAll`）——原样复制。
3. 中间放入所选图型的 fig 块，把示例数据替换为真实数据。

## 2. fig() 数据替换契约

```js
fig({
  n: 10, t: '环上的牵连', sub: 'Chord Ring · 弦环',
  note: '一句话数据旁白（写关键数字结论）',
  draw(w, h) {
    // ① 真实数据内嵌此处（数组/对象字面量）
    // ② 复用画廊同编号图型的几何计算代码，仅换数据源
    // ③ 最强通路/最高值 → ACC 珊瑚强调；其余走 inkA() 阶梯
  },
  init(el) { /* 可选：挂 hover/拖拽 */ }
})
```

注意：
- 数据量以 30 行以内聚合结果为宜；大表先聚合再进 `draw`。
- 强调逻辑（哪条通路加粗、哪个节点珊瑚）须按真实数据重算，不沿用示例的硬编码索引。

## 3. 主题切换页必须包含

- 三按钮 MONO / INK / CORAL（或至少默认 mono + 说明）。
- 切换函数：重赋全局色变量 → `applyPaletteVars()` → `renderAll()`。
- 自检：三主题各重绘一次，无残留旧色。

## 4. 质检流程（固化）

1. `node --check`：抽出 `<script>` 内容做语法检查。
2. 桩 DOM 冒烟：node 环境 stub `document`（`createElementNS`、`querySelectorAll`、`addEventListener`、`dataset` 等），断言每个 fig 的 `draw` 执行不抛错 + 三主题 `renderAll()` 各跑一遍。
3. 浏览器预览：三主题切换、hover tooltip、REPLAY、专属交互逐一点验。
4. 交付单文件 HTML。

常见坑：
- 桩 stub 忘补 `document.addEventListener` → 直接报未定义。
- 透明度色值依赖具体 rgb 字符串的 `.replace()` → 一律改 `inkA()/accA()` 动态生成。
- rAF 物理循环改坏 → 一次重构 `tick/render/step` 三段式，不打补丁。

## 5. 真实范例

`examples/sku-dashboard-jul2026.html`：66 行 SKU 明细（17 销售 × 4 大区 × 5 品类 × 12 SKU，总额 ¥239 万）→ 聚合后套 FIG.10 弦环（大区↔品类流向）+ FIG.19 涟漪扇（品类排档，注：该型已从画廊 v7 删除，仅存于范例）+ FIG.7 径向棒糖（SKU 排名）。
