# High-Fidelity Workflow

当用户希望“尽量一模一样”“多个产品经理做出来都别跑偏”“仅靠 skill 够不够”时，读这份文件。

## 结论先说

**只靠文字 skill，可以统一方向，但很难长期保证 100% 不漂。**  
要想让不同产品经理产出的页面持续高度还原 GOFO 管理后台，推荐使用：

`skill + 真实模板资产 + 固定复用工作流`

这份 skill 已经把真实模板资产一起打包在 `assets/template/` 里。

## 三档方案

### 方案 A：仅用 skill

适合：

- 快速出图
- 低保真或中保真原型
- 先讨论信息架构和页面结构

优点：

- 轻
- 适合直接基于 PRD 生成

限制：

- 不同代理仍可能对尺寸、层级、留白做“合理发挥”
- 越是复杂页面，漂移越明显
- 多 tab 监控页、PRD 很重的实时看板，如果没有额外配方和验收清单，最容易出现删模块、错配布局或局部交互失真

### 方案 B：skill + 模板资产复用

这是**推荐默认方案**。

做法：

1. 以 `assets/template/gofo-admin.html` 为真实起点
2. 保留顶栏、侧栏、tabs 与它们的 JS 逻辑
3. 只替换 `.gofo-content` 内的业务模块
4. 如果需要新页面，新增一个 `activeTab === 'new-key'` 模板块
5. 如果需要新菜单，补到 `const menu = [...]`

优点：

- 还原度最高
- 结构最稳定
- 不同人反复生成时最不容易漂

### 方案 C：skill + 私有脚手架 / 私有组件库

这是长期最稳的企业级方案。

推荐把下面内容沉淀成可直接复用的内部 starter：

- 固定 Layout 组件
- 固定 Menu / Tabs 逻辑
- 查询筛选卡
- 查询表格壳子
- 基础表单模板
- 高级表单模板
- 详情页模板
- 页面级 design token

优点：

- 一键起页
- PM、设计、前端共用同一套起点
- skill 只负责“选模板 + 填内容”，不会每次重造骨架

## 实操工作流

### 1. 先选模板

按 `references/page-recipes.md` 选中最接近的页面配方。

如果页面属于“多 tab 实时监控 / 站点盯盘 / 运营监控”，优先判定为 `dashboard-monitoring`，再为 tab 内部模块选择 `query-table`、`detail`、`card-list` 等子配方。

### 1.5 复杂 PRD 先做对照清单

当 PRD 明确给出了 tab、模块、弹窗、抽屉、交互入口时，先整理一份对照清单：

1. tab 原文与顺序
2. 每个 tab 下的模块名称
3. 每个模块的布局类型
4. 哪些模块继承 `query-table` / `detail` / `card-list`
5. 哪些内容允许合并，哪些内容不允许动

没有这一步时，复杂监控页最容易“看起来像做完了”，但实际漏了 PRD 定义的模块。

### 2. 再复制真实块

在 `assets/template/gofo-admin.html` 搜以下锚点：

- `activeTab === 'query-table'`
- `activeTab === 'basic-form'`
- `activeTab === 'advanced-form'`
- `activeTab === 'step-form'`
- `activeTab === 'card-list'`
- `activeTab === 'detail'`
- `activeTab === 'dashboard-basic'`
- `activeTab === 'dashboard-visual'`
- `activeTab === 'data-screen'`

复制时至少同时关注：

- 页面模板块
- 该页面 CSS
- 该页面 state / mock 数据
- 该页面 handler

如果是 `dashboard-monitoring` 这类组合页：

- 顶部筛选优先复用 `query-filter-card`
- 统计卡与图表骨架优先复用 `dashboard-basic`
- 左右分栏 workbench 优先复用 `detail` / `card-list`
- 管理型表格优先复用 `query-table`

### 3. 只在边界内改

默认只改这些地方：

- `.gofo-content` 的页面内容
- `menu` 数据
- 页面自己的 `ref` / `computed` / `handler`
- 与该页面直接相关的 CSS

不要改：

- `gofo-header`
- `gofo-sidebar`
- `gofo-tabs-bar`
- 侧栏 hover 展开收起逻辑
- tabs 收纳逻辑

### 4. 最后做还原校验

至少逐项核对：

1. 顶栏是否还是 `48px`
2. 侧栏是否还是 `55px -> 270px`
3. tabs 是否还是 `41px`
4. 内容区 padding 是否还是 `16px`
5. 菜单是否仍然单父级展开
6. 页面内部是否使用了正确配方
7. 列表是否仍然走 `VxeTable`
8. 纯图标按钮是否都带 Tooltip
9. PRD 明确写出的 tab / 模块 / 交互入口是否逐项落地
10. `Modal` / `Drawer` 是否首屏可用，没有被表格整体撑出屏幕
11. 看板、详情页、弹窗里凡是出现“筛选 + 表格”，是否仍保留 `query-table` 的工具栏结构

## 如果用户问“只用 skill 做不到怎么办”

直接给出下面建议，不要硬撑：

1. **优先建议用这份 skill 自带的真实模板资产。**  
   这比纯文字规则稳定得多。
2. **再往前一步，做内部 starter repo 或私有组件库。**  
   让每次生成都从 GOFO 壳子起步，而不是从空白页起步。
3. **如果是高要求页面，建议同时提供截图或页面样板。**  
   skill 负责规则，截图负责视觉锚点，二者叠加最稳。

## 这份 skill 的最佳使用姿势

最推荐的提示方式是：

- 给 skill
- 给 PRD
- 明确页面类型
- 明确“固定 L 型框架不可改，只允许替换内容区”

如果用户明确说“从 0 开始”“不要记住上下文”“基于最新 skill 和 PRD 重建”，就不要参考之前生成过的 HTML 结果，直接回到 PRD、skill 和模板资产重新判型与拼装。

如果用户没说这句，也要默认按这个约束执行。
