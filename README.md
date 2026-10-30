# 砚笺 YanJian · 思源笔记主题

> **同一间书房的两种时辰——浅色是日光下的宣纸，深色是夜灯旁的砚台。**
> 用文房器物的克制配色与中文优先的排版，让界面退成纸，让块与双链成为墨迹。

- **笺（浅色）**：米黄纸底 `#FAF7F1` + 青花靛青 `#2F5D8A` + 朱砂 `#B3362A`
- **砚（深色）**：暖墨底 `#26241F` + 灯下亮青 `#82A7CC` + 灯照朱砂 `#E07B6D`

两套色板**各自独立设计**，不是反相、不是同色相换明度。

---

## 一、安装

主题为纯 CSS 实现（无 `theme.js`），适用于思源 **v3.8.0 及以上**的桌面端与移动端（更低版本会被 `minAppVersion` 拦下，见「版本要求」）。

### 方式 A：手动安装（推荐首次使用）

1. 打开思源 → `设置` → `外观` → 在「主题」区块点击**打开主题文件夹**
   （或直接前往工作空间目录：`{工作空间}/conf/appearance/themes/`）。
2. 在本目录下新建文件夹 **`YanJian`**（名称必须与 `theme.json` 的 `name` 字段完全一致）。
3. 将本目录中的 `theme.json`、`theme.css`、`theme-dark.css`、`README.md` 一并复制进 `YanJian/`
   （集市资产 `icon.png` / `preview.png` 仅集市页展示用，手动安装可不复制）。

   ```
   {工作空间}/conf/appearance/themes/YanJian/
   ├── theme.json
   ├── theme.css
   ├── theme-dark.css
   └── README.md
   ```

4. 回到思源 → `设置` → `外观` → 刷新主题列表（或重启思源）。
5. 浅色模式选「砚笺 YanJian」，深色模式同样选「砚笺 YanJian」。

目录结构示意：

```
themes/
└── YanJian/
    ├── theme.json
    ├── theme.css        ← 浅色 + 全部组件/状态/断点样式
    ├── theme-dark.css   ← 深色独立色板（由 theme.css 首行 @import 载入）
    └── README.md
```

### 方式 B：集市安装

主题上架后，`设置` → `集市` → `主题` → 搜索「砚笺 / YanJian」→ 安装 → 在外观中选用。

### 版本要求

`theme.json` 声明 **`"minAppVersion": "3.8.0"`**：思源据此拦截低于 3.8.0 的安装
（官方规则是「包未声明该字段则不拦截」，因此必须显式声明才有一道版本闸）。
样式基于 **v3.8.x** 的 DOM 与 CSS 变量体系（含 `--b3-ui-*` 密度变量、
`data-theme-mode` 属性）；更早的 3.x 客户端在集市与手动安装时都会被此字段拦下。

---

## 二、两种模式如何生效

客户端的主题机制是：**`theme.css` 恒被加载**（无论当前是浅色还是深色），
`theme-dark.css` 不会被单独 `<link>`。因此深色色板写在 `theme-dark.css`，
由 `theme.css` 第一行 `@import url("theme-dark.css");` 带入同一份样式表，
作用域是 `:root[data-theme-mode="dark"]`（官方 `util/_reset.scss` 的机制）。

**加载顺序**（G09，已按客户端模板核对）：`baseStyle`（官方 base.css）→
`themeDefaultStyle`（主题 `theme.css`）→ `${themeStyle}`。即官方基础样式先、
主题后；深色规则都带 `:root[data-theme-mode="dark"]` 前缀、特异性更高，
因此深色能完整覆盖浅色。

> 若你只用其中一种模式：把 `theme.json` 的 `modes` 改成 `["light"]` 或 `["dark"]` 即可。

---

## 三、变量体系（两层）

### 第一层：官方 `--b3-*`（兼容性承诺层）

共 **207 个**变量声明，覆盖官方 `daylight/theme.css` 的全部 **185 个** `--b3-*` 变量
（脚本比对：`missing in light: (none)`），并按官方分组排列：主色 / 文字 / 字体 /
背景 / 工具栏 / 线条 / 滚动条 / 列表 / 脑图 / 菜单 / 提示 / 遮罩 / 卡片 /
自定义文字 / 动画 / 高亮 / switch / 阴影 / 图表 / 代码块 / 行内元素 / PDF /
callout / 表格 / 数据库 / 嵌入 / 引述 / 父块。

主题升级不改名；官方新增变量时本主题不会失效（未覆盖的变量回落到官方默认值）。

> **`--b3-font-color13` 是反白专用、与背景色 13 成对使用**（官方 token 语义，
> daylight/midnight 同款）：它与 `--b3-theme-background` 同值，官方指南样例都是
> color13 + background13 成对内联。若单独把它铺在纸底上会得到同色叠同色
> （对比 1.000:1）——这是配对设计的边界，不是主题缺陷。

### 第二层：`--yj-*`（本主题自有，允许整体覆写）

| 分组 | 变量 | 值 |
|---|---|---|
| 间距 | `--yj-space-1/2/3/4/6/8` | 4 / 8 / 12 / 16 / 24 / 32 px |
| 缓动 | `--yj-ease-out`（落墨） | `cubic-bezier(.2, 0, 0, 1)` |
| | `--yj-ease-in`（顿笔） | `cubic-bezier(.4, 0, 1, 1)` |
| | `--yj-ease-spring`（回锋） | `cubic-bezier(.34, 1.3, .64, 1)` |
| 时长 | `--yj-dur-micro/base/enter/spring/pulse` | 100 / 180 / 240 / 260 / 400 ms |
| 阴影 | `--yj-shadow-card` | 暖色纸叠纸（深色为深投影） |
| 行长 | `--yj-measure` | `calc(字体px × 46)`，随断点收为 44 / 36 |
| 焦点 | `--yj-focus-ring` | 2px 间隙 + 2px 主色环 |
| 选区 | `--yj-selection-bg` | 深色选区底色（32% 主色派生；浅色走 `--b3-theme-primary-lighter`） |

**个性化覆写示例**（`设置` → `外观` → `代码片段` → CSS，或写进工作空间 `conf/custom.css`）：

```css
/* 换一个靛青主色：交互色随主色自动推导——
   按钮 hover/active、链接 hover、输入框光晕、按钮投影、switch 悬停、
   callout 淡底、深色选区底色都已改为 color-mix(主色 …)。
   仍需手动同步的是**主色刻度**四值（官方把 light/lighter/lightest 当独立变量），
   以及**语义色**（链接正文色 --b3-protyle-inline-link-color、代码/引用/callout
   的功能色）——它们不是交互色，按语义单独命名，不随主色变。 */
:root {
  --b3-theme-primary: #345F73;
  --b3-theme-primary-light: rgba(52, 95, 115, .54);
  --b3-theme-primary-lighter: rgba(52, 95, 115, .38);
  --b3-theme-primary-lightest: rgba(52, 95, 115, .12);
}

/* 更宽的编辑区（默认 46 字宽，这里放到 56） */
:root { --yj-measure: calc(var(--b3-font-size-editor, 16px) * 56); }

/* 换回官方圆角规格 6/3/12 */
:root {
  --b3-border-radius: 6px;
  --b3-border-radius-s: 3px;
  --b3-border-radius-b: 12px;
}

/* 可选「书卷」衬线正文（只影响编辑器正文，UI 保持黑体） */
.protyle-wysiwyg {
  --b3-font-family-protyle: "Source Han Serif SC", "Noto Serif CJK SC", "Songti SC", SimSun, serif;
}
```

---

## 四、状态覆盖

| 状态 | 表达 | 实现位置 |
|---|---|---|
| **悬停** | 列表/树/菜单 `--b3-list-hover` 底；行内链接变深一档（浅 `color-mix(主色 88%, #000)` = rgb(41,82,121)、深 `#9BBDDB`）；块标 100ms 淡入 | `.b3-list-item:hover`、`.b3-menu__item:hover`、`span[data-type~="a"]:hover` |
| **焦点** | `:focus-visible` 2px 靛青外环 + 2px 间隙（`--yj-focus-ring` 双环）；输入框另加 4px 淡色晕。**菜单例外（2026-10-29 用户指令）**：`.b3-menu` 全家（菜单项/子菜单/操作项）不出外扩环，键盘焦点指示改走悬停同款底色通道（`--b3-list-hover` 底 + on-background 文字，背景变化即指示，可访问性不丢）；当前菜单项只有 700 字重（D9 起：左缘墨条与 48% 内框均已移除）；`.b3-menu__action` 保留官方 1px inset outline 作焦点指示；禁用顶栏按钮（`toolbar__item--disabled` / `:disabled`）压掉外扩环（WCAG 2.4.7 对禁用控件不作要求） | 见「焦点」选择器组（`.b3-menu__item` / `.b3-menu__action` 已从环清单剔除）与 `.b3-text-field:focus`；菜单底色通道见 `.b3-menu__item:focus-visible` |
| **按下** | `translateY(.5px)` + 主色加深/淡色底，文字不变色（对比度不塌） | `.b3-button:not(:disabled):active` 等 |
| **禁用** | 文字落 `--b3-theme-on-surface-lighter`，去阴影去位移，`cursor: not-allowed`，不用 opacity（开关/下拉/工具栏/菜单项另压掉官方的 `.38`/`.68`/`.54` 透明度，菜单子项用 `:not(.b3-menu__submenu)` 叠权重） | 各 `:disabled` / `[disabled]` |
| **触屏行级动作** | `(hover: none)` 下 `.b3-list-item--hide-action` 的更多按钮常显——触屏没有 hover，否则该入口永久不可达。浅色 on-surface 全不透明（有效对比 4.62/选中行 3.91，Z1-02：原 on-surface-light × .75 只有 2.23/1.99 不达 1.4.11 的 3:1），深色 on-surface-light × .75（4.22/3.76，≥3）；按下提满 | `@media (hover: none)` |
| **输入悬停** | 描边 `on-surface-light`（浅 3.40:1）/ `on-surface`（深 5.67:1），线宽统一 1px；`--text` 变体改用极淡底纹（保持无框形态） | `.b3-text-field:hover`、`.b3-select:hover` |
| **页签当前态** | 靛青 12%/16% 底 + 字重 500（D9-2 起：1px 内框与 2px 底墨条均已移除）；当前页签宽度放宽到 min(420px,42vw)，其它页签仍 176px | `.layout-tab-bar .item--focus` |
| **加载** | 主色旋转 1s linear；按钮进度条用主色淡底推进 | `.fn__rotate`、`.b3-button--progress` |
| **编辑中** | 当前块 2px 淡靛整圈描边（官方做法，D9 起；原「研墨条」左缘实线+8% 底已按用户指令移除） | `.protyle-wysiwyg--navigation` |
| **选中块** | 12%（深 16%）靛青底 + 1px 靛青细框 | `.protyle-wysiwyg--select::after` |
| **自动保存** | 顶栏 `#barSync` 图标 2s 呼吸（循环动画仅两处之一，另一处是加载旋转 `.fn__rotate`） | `#barSync svg.fn__rotate` |
| **保存成功** | snackbar 1→1.15→1 回锋脉冲 400ms | `.b3-snackbar__content` + `yj-save-pulse` |
| **保存失败** | 朱砂淡底横幅 + 1px 朱砂边 + 朱砂文字，不用深底（避免与成功态混淆）；横幅内重试按钮文字用 `--b3-card-error-color`（深色 6.55/浅色 6.10），hover 叠 12%/18% 朱砂并保留 inset 描边 | `.b3-snackbar--error` |
| **空状态** | `--b3-empty-color` 文字、行高 1.75；配一枚 24px 淡墨「砚池」圆点（CSS 伪元素，不新增 DOM） | `.b3-list--empty`、`.protyle-wysiwyg--empty:empty::before`、`.search__empty` |
| **搜索当前项** | 藤黄当前条 `#E9C77E`（深 `#5A5340`） | `.search__list .b3-list-item--focus` |
| **移动外壳** | 顶栏/底栏/抽屉/遮罩补上主题色：底栏悬浮胶囊、抽屉面纸亮一档 + 单侧砚沿、遮罩用蒙纸色；抽屉图标与面包屑按钮抬到 ≥44px | `.mobile-topbar`、`.mobile-bottom-bar`、`.side-panel`、`.side-mask`、`.protyle-breadcrumb__icon` |
| **触控按下** | `(hover: none)` 时关闭全部 hover 底色，改为 `:active` 即时反馈 | `@media (hover: none)` |
| **减少动效** | `prefers-reduced-motion: reduce` 下动效近零，保存呼吸改静态半透明 | 同媒体查询 |

### 焦点可达性实测注记（2026-10-29，3.8.8 桌面真机）

- **外扩环保留项**：`.b3-button`、`.toolbar__item` 等非菜单按钮类——真机键盘可达、渲染正常（2026-10-29 真机复核通过）。
- **菜单类（`.b3-menu__item` / `.b3-menu__action`）不出外扩环**（2026-10-29 用户指令）：思源在打开子菜单、恢复菜单等流程对菜单项调 `.focus()`，外扩环曾随之出现；且菜单键盘主路径（ArrowUp/Down 导航）只搬 `--current` 类、不搬 DOM focus，指示由 `--current` 悬停同款底色承担。
- **3.8.8 桌面键盘不可达（`:focus-visible` 规则保留作防御、真机不生效，防后续误报回归）**：dock 项（span）、文档树项（li）、状态栏项（div）、`.b3-list-item__action`、页签 `.item`（li，tabIndex=-1；Tab 遍历 12 站无一落在页签）——这些区域的焦点提示是思源应用层设计（靠选中态），主题无法补救。
- **未实测**：块手柄 `.protyle-gutters button` 仅在悬停块时插入 DOM，本轮探针未捕获实例——按非菜单原生 button 暂判保留外扩环。
- 暗色焦点规则与浅色逐条镜像同改；暗色真机复核留人工（3.8.8 内核无 `/setting/getAppearance` API，无法编程切换外观模式，请按「设置 → 外观 → 深色」后 Tab 走一遍主菜单/顶栏复核）。

### 动效（全部绑定块级操作语义，无装饰动画）

| 场景 | 规格 |
|---|---|
| 颜色/背景/悬停 | 100ms 落墨 |
| 面板折叠、页签、菜单进场 | 180ms 落墨 |
| 对话框、悬浮预览 | 240ms 落墨（缩放 0.98→1 / 上移 4px） |
| 块拖拽落定、闪卡 | 260ms 回锋（曲线已备，实际落定由应用逻辑触发） |
| 保存/闪卡确认脉冲 | 400ms 回锋 |
| 双链跳转落点 | 左缘线 `--yj-dur-enter`(240ms) 展开 → `--yj-dur-hold`(1.5s) 停留 → `--yj-dur-pulse`(400ms) 顿笔淡出 |
| 加载 / 同步呼吸 | 1s linear / 2s ease-in-out（仅状态指示） |

### 组件特化

- **引文笺**：块引用 = 左缘 2px 黛紫竖线 + 8% 黛紫底，悬停竖线加粗至 3px（官方默认是虚线下划线，本主题以 `background-image: none` 覆盖）。
- **碑帖**：代码块 5% 墨底（深 7% 暖白）。
- **笺注**：Callout 左缘色条沿用官方结构，加对应的功能色淡底（6–12%）。
- **铅笔稿**：AI 面板 / AI 消息 = 3% 墨底 + 1px 虚线边。
- **编辑中**：当前块 2px 淡靛整圈 `outline`（`primary-lighter`，`outline-offset:-2px`，无底色）——D9 起对齐官方；原「研墨条」（左缘实线 + 8% 底）已按用户指令移除，与「选中块」（有外框、底色略重）在明度上区分。

---

## 五、排版（中文优先）

- **字体栈**：拉丁字体排在 CJK **之前**，西文自然落到拉丁字体、汉字穿透到中文字体。
  `BlinkMacSystemFont → Helvetica Neue → PingFang SC → HarmonyOS Sans SC → MiSans →
  Noto Sans CJK SC → Source Han Sans SC → Microsoft YaHei → arial`。
  并按 `:root:lang(zh-CN / zh-TW / ja)` 提供分语言变体（官方 daylight 同款挂钩）。
- **行高**：正文（含列表项内段落、引用内段落、表格单元格）统一 **1.75**（覆盖官方 1.625）；
  列表块标高度同步改为 `calc(1.75em + 8px)`，标题在列表内按官方「分频」算法折算。
- **标题**：字号沿用官方 1.75 / 1.55 / 1.38 / 1.25 / 1.13 / 1 em；H1–H2 字重 700 + `letter-spacing: .02em`，H3–H4 600，H5–H6 500。
- **行长**：默认 46em（@16px ≈ 736px，约 40–45 字），由 `--yj-measure` 控制并映射到官方的
  `--b3-protyle-padding-left/right`。
- **数字**：数据库单元格、表格、状态栏计数启用 `font-variant-numeric: tabular-nums`。
- **已知边界**：CSS 无法在中文与西文之间自动插入空格（「盘古之白」），`text-autospace`
  在 Electron 稳定可用前不启用；本主题不注入 hack，依赖字体栈次序与 1.75 行高保证混排可读。

---

## 六、断点

| 断点 | 策略 |
|---|---|
| **默认 > 1200px** | 行长 46em；完整 hover/焦点体系；对话框上限沿用官方 |
| **≤ 1200px** | 行长 44em（与 DESIGN §8「≥1200px 用 46em」一致；实测 1201px = 736px、1200px = 704px） |
| **≤ 1280px** | 对话框 ≤ 720px；悬浮预览 72vw；面包屑省略宽度 min(240px, 42vw) |
| **≤ 1024px** | 对话框 ≤ 640px；悬浮预览 80vw；dock 图标内距收紧；页签上限 176px；状态栏消息 46vw |
| **≤ 768px（且 `body:has(#mobileTopBar)` 移动外壳）** | 行长 36em（`max(12px, (100%−36em)/2)` 居中，390px 手机仍为 12px）；正文 15px；`#editor` 底部 80px 纸色渐隐（正文不钻到悬浮底栏/状态栏下面）；触控目标 ≥ 44×44（列表项/工具栏/dock/块标/闪卡按钮/全部 `.b3-button` 含对话框确认与删除/输入框与下拉/面包屑条目与按钮/侧栏与面板工具图标/页签项与关闭钮/拖动把手/提示条关闭钮与重试钮；行内标签按 WCAG 2.5.5 inline 豁免不撑高）；对话框 96vw；snackbar 抬到底栏+状态栏之上；面包屑弹性上限 min(240px,42vw)；抽屉展开/折叠箭头补足 44×44；隐藏移动端标题左侧图标；隐藏桌面顶栏文字、数据库字号 90% |
| **(hover: none)** | 关闭所有 hover 底色，改为 `:active` 即时反馈（两者均需 `!important`，因触控点击同时命中 `:hover` 与 `:active`）；块标常显；关闭 tooltip 伪元素 |
| **(prefers-reduced-motion: reduce)** | 动效近零**且 `animation:none`**：保存呼吸改静态半透明、印章圆点静止、双链落点线常显（原来只压 duration，`fill:both` + 1.5s delay 会让线在 2.2s 后消失；标题/列表等 `background-image` 实现同理，只压 duration 会瞬间给到 `alpha:0`） |

> 断点说明：官方桌面端唯一样式断点是 `max-width: 750px`（`app/src/assets/scss/util/_responsive.scss:1`），
> 移动端是独立结构。任务要求 1280 / 1024 / 768 三档，因此行长切换点在 **1200px**、
> 尺寸收窄在 **1280 / 1024**、移动排版在 **768**（唯一边界，官方 750px 兜底规则已并入，
> 以消除 751–768px 的双规则混合带）。
> 编辑器横向留白由官方 container query（`.protyle-content[data-padding-mode="responsive"]`）驱动，
> 主题以更高特异性的 `--b3-protyle-padding-*` 覆盖为「居中列 + 行长约束」，
> 最小边距走 `--yj-space-4`，**不是**固定 16px 内距。
> 浅色与深色两个文件各自带一份相同的行长度量规则：`theme-dark.css` 需能独立加载
> （`main-dark.html` 等深色夹具与 dark-only 用法），若只写在 `theme.css` 里，
> 深色模式会退回官方 760px 容器宽度。

---

## 七、预览页（`preview/`）

`preview/` 目录存放 12 个静态预览页（main / states / mobile / focus / menu / hover 各浅深两页），用真实思源 DOM 结构（`.layout` / `.dock` / `.layout-tab-bar` /
`.protyle` / `.protyle-wysiwyg` / `.b3-list` / `.b3-snackbar` / `.mobile-topbar` …）验证主题是否真的命中。
它们不是主题的一部分，安装时不需要复制。

| 文件 | 内容 |
|---|---|
| `main-light.html` | 桌面三栏长文（浅色）：dock + 文档树 + 页签 + 编辑区 + 大纲/反链 + 状态栏 |
| `main-dark.html` | 同一篇长文（深色，夹具只加载 `theme-dark.css` 以验证深色自足） |
| `states-light.html` | 七状态卡片：编辑中/自动保存、已保存、保存失败+重试、搜索无结果、空白新笔记、标签选中、空反链列表 |
| `states-dark.html` | 同上（深色） |
| `mobile-light.html` | 390px 独立移动外壳（浅色）：顶栏 + 抽屉 + 遮罩 + 悬浮底栏 + 页签总览 |
| `mobile-dark.html` | 同上（深色） |
| `focus-light/dark.html` | 键盘焦点演示：当前页签真实焦点 + 三处静态复刻（树项/工具栏/输入框） |
| `menu-light/dark.html` | 真实菜单 + 已展开子菜单（官方摆位公式） |
| `hover-light/dark.html` | 列表/按钮/块标/菜单四类元素静止与悬停并排 |

### 未实现清单（v1.0 如实声明）

以下 DESIGN §6.2 曾承诺的效果**没有实现**，原因是主题侧无法可靠绑定，需应用层配合：

| 效果 | 为什么没做 |
|---|---|
| 块拖拽落定 1.00→1.02→1.00 | 思源 drop 后不会再给目标块加可识别的类（`protyle/wysiwyg/index.ts` 只有 `--select` / `--hl` / `--attr` / `--hiderange`），主题无法判断"刚放下"。已改为给**选中落定**做 1→1.008→1 的微脉冲（同样是回锋曲线、同样是块级操作语义）。 |
| 闪卡 rotateY 180° 翻面 | 复习容器没有翻面状态类；翻面时机完全由应用 JS 控制。 |
| 文档树子节点 +4px 下移 | 已用官方 `.protyle-wysiwyg--animate` 实现编辑器侧渐显；**文档树**节点展开由 `b3-list-item__arrow` 旋转表达，未加位移动画（官方无展开态类可挂）。 |
| 主题亮暗切换全局过渡 | **已实现**：`body` + 主要容器只过渡 `color`/`background-color` 180ms（不写 `all`）。清单必须含真正绘纸面的 `.protyle` / `.layout__center .layout-tab-container`，且标题等文字自身也要有 color 过渡——否则纸面渐变而文字瞬时跳变，翻转帧会出现半张纸、标题只剩 1.87:1。 |

配套文件：

- `preview-base.css` — 按官方 SCSS 逐段复刻的**脚手架**（官方基础样式不随主题分发）。
  主题文件在其后加载，因此主题覆盖真实生效。这个文件不属于主题。
- `assets/icons.svg.html` — 官方 litheness 图标 sprite 的 symbol 子集，内联到各页，避免任何外部请求。
- `verify.js` — 校验脚本（需本机 `playwright-core` + Chromium；路径可用 `YJ_PLAYWRIGHT_PATH` / `YJ_CHROMIUM_PATH` 覆盖）：检查 6 个无脚本预览页（main/states/mobile 浅深）无外链/无脚本、
  主题色生效、行宽 = 46/44/36em、移动端触控目标 ≥44px；focus/menu/hover 六页含演示脚本，
  由 `preview/review/verify-r3.js` 单独门控。命令：`node preview/verify.js`。

页面零外部资源：字体走系统栈，图标内联，无 `<script>`。

---

## 八、目录与文件说明

| 文件 | 作用 |
|---|---|
| `theme.json` | 主题清单：`name`（须与文件夹名一致）、`author`、`url`、`version`、`minAppVersion`、`frontends`、`modes`、`displayName`、`description`、`readme`、`icon`、`preview`、`keywords` |
| `theme.css` | 浅色 `:root` 全套变量 + 全部组件/状态/动效/断点；首行 `@import` 深色文件 |
| `theme-dark.css` | `:root[data-theme-mode="dark"]` 深色独立色板 + 对应组件覆盖（含独立的行长度量规则） |
| `icon.png` / `preview.png` | 集市展示资产（图标 160×160、预览 1024×768，尺寸对齐官方 theme-sample），仅集市页使用，运行不需要；由 `tools/gen_market_assets.py` 从预览源图生成 |
| `README.md` / `README.en.md` | 中文 / 英文说明（`theme.json` 的 `readme.zh-CN` / `readme.default`） |

> `preview/` 为开发期验证页，**不参与主题安装**。安装时只需复制 `theme.json`、`theme.css`、
> `theme-dark.css`、`README.md`（集市安装会自动带上 `icon.png` / `preview.png`，见第一节）。

---

## 九、打包与发布（维护者）

```bash
python tools/gen_market_assets.py   # 先从 preview/shots/main-light-1440.png 生成 icon/preview
python tools/package.py             # 按白名单产出 dist/package.zip（文件置于 zip 根）
```

- 白名单：`theme.json`、`theme.css`、`theme-dark.css`、`README.md`、`README.en.md`、
  `LICENSE`、`icon.png`、`preview.png`。`SYNC-LOG.md`、`tools/`、`reports/`、`preview/`、
  `design/` 与任何探针/日志都不会进包。
- `package.py` 自检：`theme.css` 首行 `@import` 目标必须在包内、`theme.json` 的
  `icon`/`preview`/`readme` 必须指向包内文件、`icon.png`/`preview.png` 的 sha256 必须
  等于 `tools/market-assets.json`（即最近一次生成脚本的输出）。
- 发布流程：把 `dist/package.zip` 作为 Release 资产上传，再向
  [siyuan-note/bazaar](https://github.com/siyuan-note/bazaar) 提 PR。
  `theme.json` 的 `url` 必须填**公开仓库地址**；本仓库尚无公开地址，因此该字段暂为空，
  集市 PR 前必须补上（不能编造）。

---

## 十、许可

MIT，见仓库根 `LICENSE`。主题为纯 CSS：不执行脚本、不写配置、不发起网络请求。

---

## 十一、卸载

`设置` → `外观` → 切换回 `daylight` / `midnight`，然后删除
`{工作空间}/conf/appearance/themes/YanJian/` 整个目录即可。
主题不写入任何配置、不执行脚本，删除后无残留。
