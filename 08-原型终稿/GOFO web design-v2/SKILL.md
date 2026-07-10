---
name: "gofo-web-design-v2"
description: "基于真实 GOFO 管理后台模板生成统一的 Web/PC 企业后台页面。当用户要求按 GOFO 风格创建或还原 B 端页面、固定 L 型框架、查询表格、表单页、详情页、结果页、数据看板、复合监控看板，或希望多个产品经理依据 PRD 输出一致页面风格时调用。尤其适用于多 tab 监控页、PRD 驱动的实时看板、需严格复用 GOFO 模板骨架的场景。"
---

# GOFO Enterprise Web Design Skill

这份 skill 的目标不是“画一个像 GOFO 的后台”，而是**让不同产品经理或代理在使用同一份 skill 时，输出同一套 GOFO 管理后台骨架、节奏、组件关系和页面模块**。  
**唯一真实来源**是 `assets/template/gofo-admin.html`，而不是口头描述。

## 解释与优先级

- `MUST`：强制要求。无明确业务例外时不得偏离。
- `SHOULD`：默认推荐。偏离时应有明确理由，并优先检查是否已有更接近的模板块可复用。
- `MAY`：可选能力。仅在用户需求明确出现时启用，不主动加戏。
- `FORBIDDEN`：明确禁止。不得以“优化”“更现代”“更通用”为理由替换。

当规则发生冲突时，按以下优先级解释：

1. `assets/template/gofo-admin.html`
2. 当前 `SKILL.md` 中的硬规则与红线
3. `references/immutable-layout.md`、`references/page-recipes.md`、`references/component-contract.md`、`references/high-fidelity-workflow.md`
4. 本文件中吸收自补充全量规范的附加规则
5. 示例代码、实现片段、说明性文字

同级规则若仍冲突，遵循两个原则：

- 更具体的页面级规则覆盖更一般的组件级规则
- 限制更强、自由度更低的规则优先

## 先读什么

1. **总是先读** `references/immutable-layout.md`  
   这里定义固定不变的顶栏、侧栏、页签栏、内容区与菜单交互合同。
2. 用户要生成具体页面时，再读 `references/page-recipes.md`  
   这里把真实模板里的典型页面做成了可复用“页面配方”。
3. 需要补精确样式、组件细节或交互节奏时，再读 `references/component-contract.md`。
4. 用户要求“高度还原”“一模一样”“如果 skill 不够怎么办”时，再读 `references/high-fidelity-workflow.md`。

## 核心原则

1. **固定框架不能重画。** 顶栏、侧栏、页签栏、主内容布局必须复用 GOFO 模板的结构、类名、尺寸和交互逻辑。
2. **优先复制模板，禁止重新发明骨架。** 如果目标页面能在模板里找到相近块，先复用已有块，再替换文案、字段、数据、图表。
3. **只在允许区域动手。** 默认只改 `.gofo-content` 内的业务内容、`menu` 数据、对应页面 state / handler、与该页面直接相关的 CSS。
4. **模板优先于抽象规范。** 文字规范和页面配方如果与 `assets/template/gofo-admin.html` 冲突，以模板为准。
5. **生成新页面时，先判型。** 不要直接写页面。先把 PRD 归类到“查询表格 / 基础表单 / 高级表单 / 分步表单 / 卡片列表 / 详情页 / 结果页 / 常规看板 / 复合监控看板 / 大屏”之一，再套对应配方。
6. **PRD 明确写出的 tab、模块、功能，先列清单再实现。** 默认逐项映射，不得擅自删减、合并、改名；如确需简化，必须先得到用户确认。

## 绝对红线

- `FORBIDDEN` 把 GOFO 的固定 L 型骨架替换成自创布局。
- `FORBIDDEN` 把左侧 hover 展开菜单改成常规常驻 200px 菜单，除非用户明确要求改框架。
- `FORBIDDEN` 用 Ant Design Vue 的 `Table` 取代模板里的 `VxeTable` 列表页实现。
- `FORBIDDEN` 把图标按钮裸放出来，纯图标按钮必须带 Tooltip。
- `FORBIDDEN` 为了“更现代”而统一改圆角、间距、标题层级。真实模板本身存在 2px 与 8px 的混用，要按页面配方复现。
- `FORBIDDEN` 把页面内容直接塞进 header、sidebar、tabs 容器里。
- `FORBIDDEN` 擅自删减、合并、改名 PRD 已明确给出的 tab、模块、交互入口。
- `FORBIDDEN` 在复杂监控页里把“筛选区 / 表格工具栏 / VxeTable / 分页”混成一个模糊模块。

## 页面类型速查

| 需求类型 | 复用 key | 搜索锚点 |
|---|---|---|
| 固定框架空壳 | `foundation` | `activeTab === 'foundation'` |
| 常规数据看板 | `dashboard-basic` | `activeTab === 'dashboard-basic'` |
| 复合监控看板 | `dashboard-monitoring` | 组合 `dashboard-basic` / `query-table` / `detail` 配方 |
| 强视觉工作台 | `dashboard-visual` | `activeTab === 'dashboard-visual'` |
| 数据大屏 | `data-screen` | `activeTab === 'data-screen'` |
| 基础表单 | `basic-form` | `activeTab === 'basic-form'` |
| 高级表单 | `advanced-form` | `activeTab === 'advanced-form'` |
| 分步表单 | `step-form` | `activeTab === 'step-form'` |
| 查询表格 | `query-table` | `activeTab === 'query-table'` |
| 卡片列表 | `card-list` | `activeTab === 'card-list'` |
| 详情页 | `detail` | `activeTab === 'detail'` |
| 成功/失败/异常 | `success` / `fail` / `403` / `404` / `500` | `result-page-card` |

## 推荐工作流

1. 先判断用户要的是“新页面”、“现有页面变体”还是“空框架”。
2. 如果 PRD 明确给出了 tab、模块、明细弹窗、抽屉、交互入口，先整理一份一一对应清单，后续按清单实现。
3. 打开 `assets/template/gofo-admin.html`，通过上面的锚点找到最接近的模板块。
4. 保留固定壳层，优先复制接近的整段模板。
5. 只替换业务字段、文案、表头、卡片内容、图表数据、抽屉或弹窗内容。
6. 如果页面需要新菜单项：
   - 在 `const menu = [...]` 中添加 key
   - 保持现有父子结构与单父级展开逻辑
   - 页面内容对应新增 `activeTab === 'new-key'` 模板块
7. 如果 PRD 没有完全匹配的页面类型：
   - 列表型优先从 `query-table` 或 `card-list` 演化
   - 编辑型优先从 `basic-form` 或 `advanced-form` 演化
   - 审批/物流链路/信息汇总型优先从 `detail` 演化
   - 分阶段流程优先从 `step-form` 演化
   - 运营总览优先从 `dashboard-basic` 或 `dashboard-visual` 演化
   - 多 tab 实时监控、站点盯盘、运营监控优先从 `dashboard-monitoring` 演化

## 复合监控看板补充规则

当 PRD 明确是“概览 + 多个业务监控 tab”的实时盯盘页面时，按 `dashboard-monitoring` 处理。

- tab 顺序、tab 文案、模块名称默认严格沿用 PRD 原文。
- 每个 tab 视为独立布局系统，不要把概览的排布机械复用到库内、派送、考核等 tab。
- tab 已经承担主标题时，内容区默认不要重复再放同名标题，除非 PRD 明确要求。
- 模块边界必须清晰，优先使用分组卡片、双栏 workbench、列表-详情结构，不要出现大面积无意义留白。
- 模块中只要出现“筛选 + 表格”，就继承 `query-table` 规则；只要出现“左侧列表 / 右侧详情”，就继承 `detail` 或 `card-list` 的分栏模式。
- 若真实模板没有 100% 对应整页，允许新增一个业务 `activeTab`，但必须用现有配方拼装，不能靠自由发挥删模块。

## 高度还原时的处理方式

如果用户要“尽量一模一样”，不要只输出抽象说明。  
直接把 `assets/template/` 里的真实模板当作起点来改，尤其是：

- `assets/template/gofo-admin.html`
- `assets/template/assets/`
- `assets/template/fonts/`
- `assets/template/vendor/`

必要时把真实模板整体复制进目标工程，只替换 `.gofo-content` 内的业务区域。

## 大文件搜索提示

`assets/template/gofo-admin.html` 很大，优先用这些搜索词定位：

- `class="gofo-header"`
- `class="gofo-sidebar"`
- `const menu = [`
- `const isMenuActive =`
- `activeTab === 'query-table'`
- `activeTab === 'basic-form'`
- `activeTab === 'advanced-form'`
- `activeTab === 'step-form'`
- `activeTab === 'card-list'`
- `activeTab === 'detail'`
- `query-table-toolbar`
- `advanced-form-section-card`
- `result-page-card`

## 输出要求

- 输出页面风格必须与 GOFO 管理后台保持同一视觉体系与框架体系。
- 当用户给 PRD 时，产出内容必须**优先落到现有页面配方上**，而不是从零发挥。
- 当用户给的是明确 PRD，交付前必须能回答“PRD 里的每个 tab / 模块 / 交互入口在页面哪里实现了”。
- 除非用户明确要求改框架，否则：
  - 顶栏、侧栏、页签栏一律保留
  - 顶栏高度、侧栏收起/展开宽度、菜单展开方式、内容区边距不得擅改
- 如果只靠 skill 约束还不足以达到高还原，应主动切换到“skill + 模板资产复用”的工作流，并明确说明这是推荐方案。

## 补充实现基线

以下内容用于补强执行一致性；若与上文、`references/` 或真实模板冲突，仍以上文与模板为准。

### 技术栈补充

- Vue 项目默认基线：`Vue 3.3+` + `Ant Design Vue 4.x` + `VxeTable 4.6.x` + `ECharts 5.x`
- Vue 工程内优先通过 `ConfigProvider` 配置主题；纯 HTML 原型也要在视觉上对齐同一套 GOFO token
- Ant Design Vue 4.x 语境下，`Modal` / `Drawer` 的显隐优先使用 `open` / `v-model:open`
- 图标继续统一走 `<gofo-icon>`，底层仅使用线性风格图标
- 补充规范里的全局尺寸、圆角、间距如果与真实模板数值不一致，不做全局覆盖，继续按模板与页面配方复现

### 最小视觉摘要

以下摘要用于让不同产品线在只读主入口时也能快速对齐同一套 GOFO 视觉基线；完整细节继续以 `references/immutable-layout.md` 与 `references/component-contract.md` 为准。

- 品牌主色：`#fc4c02`
- Hover 色：`#f76121`
- Active 色：`#e84702`
- 选中浅底：`#fff1e6`
- 顶栏背景：`#001529`
- 页面背景：`#f0f2f5`
- 顶栏高度：`48px`
- 侧栏收起/展开：`55px / 270px`
- 页签栏高度：`41px`
- 内容区默认 padding：`16px`
- 基础骨架变量中的控件/卡片圆角：`2px`
- 页面内部允许按配方混用 `8px` 圆角，但不能全局一刀切覆盖
- 正文字体：`Roboto` + 系统回退
- 局部强调字体：`Inter` 仅在模板已有位置复用，不主动扩大使用范围
- 图标风格：`<gofo-icon>` + 线性风格，不混入其他视觉体系

### 强视觉页面特例

- 仅在 PRD、页面说明或口头需求里明确出现 `强视觉`、`视觉工作台`、`驾驶舱`、`氛围感首页` 等描述，或页面配方已判定为 `dashboard-visual` 时启用；否则继续使用默认骨架圆角规则
- 启用后必须直接复用 `assets/template/gofo-admin.html` 中 `activeTab === 'dashboard-visual'` 的视觉语言与模块关系，不要额外发明另一套“强视觉”风格
- 主题色必须继续使用 GOFO 橙 `#fc4c02`；允许沿用现有 hover / active / 浅底衍生色，但不允许替换主色相
- 放大圆角只作用于 `.gofo-content` 内的视觉模块，不影响 Header / Sidebar / Tabs / 固定骨架 / 表格框架
- 强视觉页面推荐只保留两档卡片圆角：普通内容卡 `16px`，主视觉 Hero / 排名 / 核心摘要卡 `20px`
- Badge / Tag / 胶囊按钮可使用 `999px`，用于呼应页面整体圆润感
- 输入框、筛选表单、表格、分页、弹窗内基础控件仍沿用默认组件圆角，不跟随主视觉卡片统一放大
- 同一强视觉页面不要混用超过两种卡片圆角，也不要把所有组件都改成大圆角

### 组件与交互决策补充

- `Drawer`：优先用于只读详情、日志、预览、高级筛选、列设置、需要边看背景边编辑的场景
- `Modal`：优先用于短表单、一次性确认、字段较少且不需要长链路回看或复杂滚动的场景
- `New Page`：超过两步的复杂流程，不要硬塞进 `Modal`
- `Popconfirm`：用于行内轻量级二次确认；高风险操作或需要补充说明时改用 `Modal`
- 页面级主切换使用卡片式 `Tabs`；`Segmented` 仅用于视图模式或短维度切换，不能替代主 `Tabs`
- `Descriptions` 用于结构化详情摘要，`a-result` 用于成功/失败/异常页，`a-empty` 或 GOFO 缺省资产用于空状态
- `Collapse` 默认按手风琴思路处理，除非所选模板块本身定义了不同交互

### 一致性补充

- 如果所选模板块已经包含 `Breadcrumb`、`PageHeader`、`Filter Card`、`Toolbar`、`Pagination` 等标准区块，不要因为内容简单就删掉
- 同一用户目标只保留一个主入口，不要同时用多套控件表达同一层级切换
- 除非当前页面配方明确要求横向表单（例如当前 `basic-form` 配方），否则表单布局默认沿用所选模板块，不再额外发明新变体
- 查询筛选的展开/收起交互，优先复用现有 `query-table` 或 `advanced-form` 模板模式
- 页面级、工具栏级、页头级操作组中，主按钮最多一个；按钮位置先按“操作区类型”判定，不按单纯左侧/右侧机械推导
- 查询筛选区底部操作默认右对齐，顺序固定 `重置 -> 查询 -> 展开/收起`；`查询` 是主按钮，但其右侧允许继续跟随 `展开/收起` 这类辅助动作
- 表单底部、`Modal Footer`、`Drawer Footer` 默认右对齐，顺序 `取消 / 重置 / 上一步 -> 确认 / 保存 / 提交`；主按钮在最右
- 列表工具栏、`PageHeader`、卡片头里的文字业务操作组可以左对齐；当同组包含主次按钮时，主按钮通常放在该组末位，不强行要求最左
- 左侧业务操作并不只限于表格里的 `导出 / 导入 / 新建`；若模板已有页头、区块头、产品区顶部操作组，继续沿用原位置
- 右侧操作区优先承载图标型辅助操作或次级动作，不要把与左侧同层的文字主操作重复放两边
- 看板页、详情页、弹窗、抽屉里凡是出现管理型表格，都继续沿用 `query-table` 的工具栏与分页结构，而不是临时发明简化版表格壳子

### 状态与质量补充

- 表单校验优先使用 Ant Design Vue Form 规则，结合实时反馈与提交校验
- 页面应考虑加载态、空态、结果态，而不只生成 happy path
- 推荐至少覆盖三层 loading：页面级 `Spin`、表格级 loading、按钮级 `loading`
- 局部刷新或首次加载信息块时，可用 `Skeleton`，但不要替代真实业务骨架
- `query-table` 与承担管理列表职责的 `VxeTable` 默认必须支持 `刷新`、`列设置`、`列显隐`、`列顺序拖拽`、`列宽调整`、`左右冻结`、`全屏`
- 除非 PRD 明确要求轻量化，或该表只是只读关联子表 / 内嵌明细表，否则不要删减上述表格能力，也不要退回简化版静态表格
- 弹窗或抽屉中的表格必须优先保证首屏可用：内容区应限制在视口内滚动，必要时降低默认 `pageSize` 或收紧表格高度，但不要让弹窗整体溢出屏幕

### 输出补充

- 交付代码时，尽量同时给出：
  - 选中的页面配方
  - PRD 的 tab / 模块 / 交互清单与对应实现位置
  - `menu key` / `activeTab key`
  - 页面模板块
  - 页面相关 CSS
  - 页面 state / mock 数据 / handlers
  - `Drawer` / `Modal` / 新页面 的选型结果
  - 空态、加载态、结果态的处理方式
- 如果本次只输出原型或结构稿，要明确哪些是占位数据，哪些是不可变的 GOFO 骨架

### 任务输入要求

当用户给 PRD 或口头需求时，至少尝试补齐这些信息；缺失时先按页面配方推断，再明确写出你的假设：

- 页面类型：`query-table` / `basic-form` / `advanced-form` / `step-form` / `card-list` / `detail` / `dashboard-basic` / `dashboard-monitoring` / `dashboard-visual` / `data-screen`
- 所属菜单与页面标题：挂到哪个一级/二级菜单、tab 名称是什么
- PRD 对照清单：tab 顺序、模块名、交互入口、哪些内容允许合并或简化
- 核心信息结构：字段清单、分组结构、说明文案
- 列表型页面：表格列、筛选条件、批量操作、行内操作、分页方式；若未特殊说明，默认按管理型 `VxeTable` 启用列设置 / 显隐 / 拖拽 / 列宽调整 / 冻结 / 全屏
- 表单型页面：必填项、校验规则、提交动作、是否分组、是否需要子表
- 详情型页面：概览信息、描述信息、关联列表、流程链路、顶部操作
- 反馈与状态：空态、loading、成功/失败/异常态是否需要定制文案
- 容器选型：是否需要 `Drawer`、`Modal`、新页面，是否需要图表

如果用户没有给全，默认遵循：

- 列表管理类：优先 `query-table`
- 复杂录入类：优先 `advanced-form`
- 汇总与链路类：优先 `detail`
- 多 Tab 监控 / 实时看板 / 站点盯盘 / 运营监控：优先 `dashboard-monitoring`
- PRD 明确写了 `强视觉` 或需要直接参考模板里的强视觉工作台：优先 `dashboard-visual`，并直接调用模板强视觉风格
- 不新增框架，不改固定骨架，只在 `.gofo-content` 内扩展

### 交付前最小验收

生成任何页面后，至少自检以下项目：

1. Header / Sidebar / Tabs 的结构、类名、尺寸、交互是否仍与模板一致
2. 页面是否仍然落在 `.gofo-content` 中，而不是侵入固定壳层
3. PRD 明确写出的 tab、模块、交互入口是否逐项保留，且没有擅自删减或合并
4. 页面是否明确套用了某个现有配方，而不是从零重画骨架
5. 列表页或嵌入式管理表格是否继续使用 `VxeTable`，且保留列设置、显隐、拖拽、列宽调整、冻结、全屏等默认能力
6. 纯图标按钮是否全部带 `Tooltip`
7. `Drawer` / `Modal` / 新页面 的选型是否符合上面的决策规则
8. 是否补齐 loading、empty、result 三类状态，而不是只做 happy path
9. 新增 `menu key` / `activeTab key` 是否语义清晰，并保持现有菜单展开逻辑不被破坏
10. 若页面属于强视觉工作台，是否已复用 `activeTab === 'dashboard-visual'` 的视觉语言，且品牌橙 `#fc4c02` 未被替换
11. `Modal` / `Drawer` 首屏是否可用，是否避免了整体溢出屏幕
