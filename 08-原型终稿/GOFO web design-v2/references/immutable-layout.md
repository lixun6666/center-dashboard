# Immutable Layout Contract

这份文件定义 **GOFO 管理后台固定不变的骨架合同**。  
任何页面生成、还原、改版，都必须先满足这里的约束。

## 真实来源

唯一来源：`assets/template/gofo-admin.html`

优先搜索：

- `:root {`
- `class="gofo-header"`
- `class="gofo-sidebar"`
- `class="gofo-tabs-bar"`
- `const menu = [`
- `const isMenuActive =`
- `const handleSidebarEnter =`
- `const handleSidebarLeave =`

## 1. 全局变量

以下值来自真实模板，优先级最高：

| 变量 | 值 | 说明 |
|---|---|---|
| `--gofo-primary` | `#fc4c02` | 品牌主色 |
| `--gofo-primary-hover` | `#f76121` | hover |
| `--gofo-primary-active` | `#e84702` | active |
| `--gofo-primary-light` | `#fff1e6` | 浅色选中背景 |
| `--gofo-header-bg` | `#001529` | 顶栏背景 |
| `--gofo-header-height` | `48px` | 顶栏高度 |
| `--gofo-sidebar-width` | `55px` | 侧栏收起宽度 |
| `--gofo-sidebar-expand` | `270px` | 侧栏展开宽度 |
| `--gofo-tabs-height` | `41px` | 页签栏高度 |
| `--gofo-border` | `#f0f0f0` | 通用边框 |
| `--gofo-page-bg` | `#f0f2f5` | 页面背景 |
| `--gofo-card-radius` | `2px` | 基础卡片圆角变量 |
| `--gofo-control-radius` | `2px` | 基础控件圆角变量 |

注意：

- 真实模板不是“全局都 8px 圆角”。
- 基础骨架变量是 `2px`，但具体页面里的筛选浮层、卡片列表、抽屉内容卡、步骤确认面板等又会使用 `8px`。
- 所以**不要全局统一覆盖圆角**，要按页面配方复现。

## 2. 顶栏合同

### 结构

固定结构：

`logo + 搜索 + 通知 + 语言 + 时区 + 用户`

### 尺寸与节奏

| 项目 | 值 |
|---|---|
| 顶栏高度 | `48px` |
| 左右内边距 | `16px` |
| 左侧组间距 | `21px` |
| 右侧组间距 | `16px` |
| Logo 宽度 | `113px` |
| 搜索框宽度 | `224px` |
| 头像 | `24px * 24px` |

### 视觉特征

- 顶栏背景固定 `#001529`
- 搜索框底色固定 `#293c52`
- 搜索框 focus 使用品牌色描边与 `rgba(252, 76, 2, 0.18)` 光晕
- 顶栏文字与图标以白色和半透明白为主
- 通知 badge 为红色圆点或红色数字角标

### 交互红线

- 搜索结果下拉挂在搜索框下方，不得改成全局搜索页
- 通知、语言、时区、账户都保持顶部 dropdown 交互
- hover 关闭延时逻辑需要保留，不要粗暴删掉全部 hover timer

## 3. 侧栏合同

### 固定行为

- 默认收起，只显示图标
- `mouseenter` 展开到 `270px`
- `mouseleave` 收起回 `55px`
- 如果当前激活的是二级菜单，展开时自动只打开它的父级
- `expandedMenus` 始终只允许一个父级展开

### 尺寸

| 项目 | 值 |
|---|---|
| 顶部偏移 | `48px` |
| 收起宽度 | `55px` |
| 展开宽度 | `270px` |
| 一级菜单高度 | `40px` |
| 二级菜单高度 | `40px` |
| 一级菜单左右 padding | `16px` |
| 二级菜单 padding | `0 24px 0 48px` |
| 底部 footer 高度 | `48px` |

### 视觉特征

- 背景白色
- 右边框 `1px solid #f0f0f0`
- 整体阴影 `0 2px 8px rgba(0, 0, 0, 0.15)`
- 一级菜单图标 `18px`
- 菜单文字 `14px`
- 文本与箭头使用 opacity / max-width 渐隐，而不是直接 `display: none`

### 激活态

- 当前一级菜单或包含当前二级项的父级菜单都视为 active
- active 样式：
  - 背景 `#fff1e6`
  - 文本主色 `#fc4c02`
  - 右侧 3px 内阴影高亮 `inset -3px 0 0 0 #fc4c02`

### 禁止项

- `FORBIDDEN` 同时展开多个一级父菜单
- `FORBIDDEN` 把二级菜单改成弹层模式
- `FORBIDDEN` 在收起状态继续保留文本占位，真实模板是完全隐藏文本并居中图标

## 4. 页签栏合同

### 尺寸

| 项目 | 值 |
|---|---|
| 总高度 | `41px` |
| sticky top | `48px` |
| 列表左 padding | `16px` |
| 卡片间距 | `4px` |
| “更多”按钮 | `32px * 32px` |
| 单个 tab 最小高度 | `40px` |
| 单个 tab 左右 padding | `16px` |

### 结构与行为

- 页签栏位于 `gofo-main` 顶部，sticky 吸顶
- 可见 tab 放在 `visibleTabs`
- 超出部分通过 `hiddenTabs` 收纳到“更多”下拉
- 新打开页面插入 closable tab
- 关闭当前 tab 时，回退到第一个剩余 tab

### 视觉特征

- 默认 tab 背景 `#fafafa`
- active tab 背景 `#fff1e6`
- active tab 文字主色 `#fc4c02`
- active tab 底边与背景同色，形成卡片连接感

## 5. 主内容区合同

| 项目 | 值 |
|---|---|
| 主区域左偏移 | `55px` |
| 内容区 padding | `16px` |
| 相邻区块纵向间距 | `16px` |
| 页面背景 | `#f0f2f5` |

规则：

- `gofo-content` 是唯一默认业务落区
- 内容区本身滚动
- 页面区块默认白卡片承载
- 除特殊页面外，统一沿用“卡片堆叠 + 16/24 间距”的节奏

## 6. 菜单数据与激活逻辑

固定菜单树：

- `foundation`
- `dashboard`
  - `dashboard-basic`
  - `dashboard-visual`
  - `data-screen`
- `form`
  - `basic-form`
  - `advanced-form`
  - `step-form`
- `list`
  - `query-table`
  - `card-list`
- `detail`
- `result`
  - `success`
  - `fail`
- `exception`
  - `403`
  - `404`
  - `500`

激活逻辑：

- `isMenuActive(item)` 对无子菜单项判断 `activeMenu === item.key`
- 对父级菜单项判断 `children.some(child.key === activeMenu)`

## 7. 允许改动的边界

默认允许：

- 在 `menu` 中新增业务页面 key
- 在 `.gofo-content` 中新增或替换页面模板块
- 增加页面自己的数据、图表实例、handler、mock 数据
- 增加该页面直接依赖的 CSS

默认不允许：

- 改 `gofo-header` / `gofo-sidebar` / `gofo-tabs-bar` 的 DOM 骨架
- 改 `handleSidebarEnter` / `handleSidebarLeave` 的展开收起模型
- 改 `visibleTabs` / `hiddenTabs` 的基本机制
- 改主题主色与固定尺寸

## 8. 交付前自检

生成任何页面后，至少检查这 8 项：

1. 顶栏是不是固定 `48px`
2. 侧栏是不是默认 `55px`，hover 才到 `270px`
3. 页签栏是不是 `41px`
4. 内容区是不是 `16px` padding
5. 菜单父级是不是单一展开
6. 当前页面是不是仍然挂在 `.gofo-content`
7. 是否保留了 GOFO 的 header / sidebar / tabs 结构和类名
8. 是否复用了真实模板页面，而不是自创骨架
