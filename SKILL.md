---
name: editorial-graph-gallery
description: 面向 AI Agent 的图谱艺术数据可视化技能：58 种编辑式图谱图型 / 7 大族（蒲公英放射 / 开扇 / 关系网络 / 流动 / 时序 / 分布 / 编辑经典），纯白纸面 + 三套可切换主题（黑白极简 / 墨蓝阶梯 / 珊瑚），输出单文件可交互动态 HTML 图表。图型包括弦环、力导向、星座连线、蜂巢、边捆绑、流场、粒子溪流、螺旋时间、山脊线、华夫点阵、年轮环等；动效分入场 / 永续 / 响应三档，支持 hover tooltip、REPLAY 重播、拖拽、点击吹散等交互，prefers-reduced-motion 自动降级。当用户要求"图谱图表 / 蒲公英图 / 开扇图 / 关系图谱 / 网络图 / 弦图 / 流场图 / 编辑式图表 / 杂志风图表 / 年报图表 / 数据艺术化"，或需要把结构化数据做成艺术化可交互 HTML 时使用此技能。
---

# Editorial Graph Gallery — 图谱艺术数据可视化（58 型）

把任意结构化数据快速转化为**编辑式图谱艺术图表**：纯白纸面、墨蓝细线、珊瑚单强调、衬线标题 + 等宽角标。58 种图型 / 7 大族，全部可交互、大部分带永续动效，输出**单文件 HTML**（零依赖，可离线打开）。

核心资产：`assets/editorial-graph-gallery.html`（画廊本体，即模板源）。真实数据实战范例：`examples/sku-dashboard-jul2026.html`。

## 工作流程（Agent 快速路径）

接到图谱类可视化请求后，按顺序执行：

1. **选图型**：读 `references/figure-catalog.md`，按数据形态选族选型——
   - 层级 / 构成 → A 放射族（旭日开扇、折扇肋、分段扇）
   - 两列实体流向 → B 开扇族或 C 关系族（弦环、二部流带、邻接点阵）
   - 网状关系 / 依赖 → C 关系族（力导向、星座连线、蜂巢、边捆绑）
   - 连续流 / 轨迹 → D 流动族（流场、粒子溪流、河曲流带）
   - 时间序列 → E 时序族（螺旋时间、日历点阵、相位钟）
   - 分布对比 → F 分布族（山脊线、蜂群漂移、镜像对比）
   - 杂志年报级小图 → G 编辑经典（点阵瀑布、区间胶囊、年轮环）
2. **读规范**：读 `references/design-system.md`，遵循纯白纸面、三主题、动效三档守则。
3. **套拆页公式**：读 `references/agent-guide.md`，从画廊抽取「前 170 行框架 + fig() 块 + 尾段挂载」组装数据页；把真实数据写进所选图型的 `draw(w,h)` 内。
4. **质检**：`node --check` 抽取脚本 → 桩 DOM 冒烟（需 stub `document.addEventListener`）→ 浏览器预览三主题切换。

## 关键约定

- **fig() 契约**：`fig({n, t, sub, note, draw(w,h), init(el)})`，数据内嵌 `draw` 内，`init` 可选（挂接交互）。
- **主题系统**：`THEMES = {mono, ink, coral}`；切换 = 重赋 `let` 全局色变量 + `applyPaletteVars()` + `renderAll()`。产出页面必须三主题全过。
- **默认主题**：mono 黑白极简；ink = #2A3B4D 透明度阶梯；coral = #C84A31 单强调。
- **动效三档**：入场（draw/pop/rise/growX/growY）、永续（sway/bob/breathe/flow/tw/fall/spin）、响应（hover/REPLAY）。
- **可访问性**：`prefers-reduced-motion` 时永续动效降级为静态定格（力导向同步沉降 50 tick）。
- **输出形态**：单文件 HTML（默认）；SVG 降级可选。

## 禁止事项

- 不引入外部 CDN / 字体 / 库——一切内联，保证离线可用。
- 不破坏三主题切换：任何新色值必须走 `inkA()/accA()` 动态函数，不得硬编码具体 rgb。
- 不改画廊原文件的图型编号与族结构；定制一律复制出独立页面再改。
