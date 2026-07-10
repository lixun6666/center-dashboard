# Component Contract

当你已经确定页面类型后，读这份文件来补页面内部的组件细节、尺寸节奏和禁忌。

## 1. 通用约束

- 图标统一走 `<gofo-icon>`，图标源限定为 Ant Design 线性风格。
- 主色固定 `#fc4c02`，hover `#f76121`，active `#e84702`。
- 列表页表格统一使用 `VxeTable`，不是 `a-table`。
- 纯图标操作必须带 Tooltip。
- `body` 字体使用 `Roboto` + 系统回退，用户名位置单独用了 `Inter`。
- 不要为了“美化”统一提高圆角和阴影，真实模板是分页面混用。

## 2. 查询筛选区

搜索锚点：

- `query-filter-card`
- `query-filter-grid`
- `query-filter-overlay`

固定规则：

- 外层卡片默认 `24px` padding
- 默认使用 `a-form layout="vertical"`
- 收起态通常 4 个字段一行
- 超过 4 个字段时使用“展开/收起”浮层补充额外字段
- 操作区始终在底部右对齐
- 顺序固定：`重置 -> 查询 -> 展开/收起`
- `查询` 是该组主按钮，但右侧允许继续跟随 `展开/收起` 这类辅助动作，不要求主按钮必须占整组最右

视觉规则：

- focus 边框与光晕使用主色
- 展开浮层挂在筛选卡片底部，底角 `8px`
- 浮层阴影比普通卡片更重

## 3. 查询表格页

搜索锚点：

- `query-table-toolbar`
- `query-table-card`
- `query-table-pagination`

固定结构：

1. 顶部筛选卡
2. 工具栏
3. `VxeTable`
4. 右下分页

工具栏规则：

- 左侧是文字业务操作组：模板里是 `导出 / 导入 / 新建`
- 当同组包含主次按钮时，通常按 `次 -> 次 -> 主` 排列，主按钮放在该组末位，不要求最左
- 右侧是图标按钮：模板里是 `删除 / 刷新 / 设置 / 全屏`
- 图标按钮尺寸固定 `32px * 32px`

表格规则：

- `border + stripe + auto-resize`
- 默认支持列显隐、列顺序拖拽、手动列宽调整、左右冻结
- 工具栏里的 `刷新 / 设置 / 全屏` 默认保留，作为管理型列表的标准能力
- 操作列默认用图标按钮
- 状态列使用“小圆点 + 文字”而不是色块大 Tag

分页规则：

- 右对齐
- 容器 `16px 24px`
- 顶部分割线

### 3.1 管理型表格的继承规则

当表格不在独立 `query-table` 页面中，而是出现在下列场景时，仍按管理型表格处理：

- `dashboard-monitoring` 的某个 tab 模块
- `detail` 页的关联明细区
- `advanced-form` 的内嵌管理区
- `Modal` / `Drawer` 中的筛选+表格区域

固定规则：

- 结构仍然是 `筛选区 -> 工具栏 -> VxeTable -> 分页`
- `导出 / 导入 / 新建` 这类文字业务操作放在工具栏左侧，不放回筛选区
- `刷新 / 设置 / 全屏` 这类图标辅助操作放在工具栏右侧
- 只要是管理型表格，就继续保留列显隐、列顺序拖拽、列宽调整、左右冻结
- 如果场景空间受限，允许减少默认 `pageSize`、限制表格 `max-height`，但不允许删掉工具栏结构

弹窗 / 抽屉补充：

- 首屏优先保证可见，不要让整个 `Modal` / `Drawer` 因表格过高而溢出屏幕
- 表格区域应内部滚动，而不是让整页或整弹窗一起被撑高
- 默认优先考虑 `5 / 10 / 20` 这类较保守的分页档位

## 4. 基础表单页

搜索锚点：

- `basic-form-card`
- `basic-form-control`
- `basic-form-textarea`

这是**单卡片、单列、左 label 右内容**的基础录入页，不是顶部 label 布局。

固定规则：

- 卡片 padding `24px`
- 使用 `label-col="{ span: 5 }"` 与 `wrapper-col="{ span: 16 }"`
- 普通输入宽 `320px`
- 文本域宽 `480px`
- 提交按钮行通过 `offset: 5` 对齐
- 尾部操作默认 `重置 / 取消` 在左，`提交` 在右

适用场景：

- 简单创建页
- 少量字段编辑页
- 目标/说明类表单
- 没有复杂分组或内嵌表格的录入页

## 5. 高级表单页

搜索锚点：

- `advanced-form-card`
- `advanced-form-section-card`
- `advanced-form-footer-actions`

这是**分段卡片 + 垂直表单 + 内嵌表格 + 详情弹窗 / 编辑抽屉**的复合页。

固定规则：

- 整页外层不再额外包白卡，使用透明背景承载多个 section card
- 每个 section card：
  - `24px` padding
  - `24px` 下间距
  - `8px` 圆角
  - 轻阴影
- section head：
  - 底部 `16px` padding-bottom
  - `1px` 分割线
  - 标题 `16px / 500`
- 表单使用 `layout="vertical"`
- 常规内容按 `a-row :gutter="24"` + 三列布局展开

附加交互：

- “更多”使用 link button，切换额外行
- 管理区用 `VxeTable`
- 新增按钮用 `dashed block`
- 详情弹窗宽 `1200px`
- 编辑抽屉宽 `520px`
- 底部主次操作右对齐，顺序 `次 -> 主`

## 6. 分步表单页

搜索锚点：

- `step-form-card`
- `step-form-steps`
- `step-form-confirm`

固定规则：

- 整页最小高度基于视窗高度计算
- 外层卡片 padding `24px`
- 步骤条最大宽 `800px`，居中
- 第一步编辑态最大宽 `800px`
- 确认态主体最大宽 `560px`
- 主要控件宽 `640px`
- 协议勾选相对 label 左偏移对齐

确认态规则：

- 警告 `Alert`
- 信息确认卡片使用分行表格式布局
- 金额使用品牌色强化显示

## 7. 卡片列表页

搜索锚点：

- `card-list-grid`
- `card-list-item`
- `courier-detail-drawer`

固定规则：

- 页面顶部同样保留筛选卡
- 卡片区使用 grid
- 默认 `4` 列，gap `24px`
- 响应式退化为 3 / 2 / 1 列
- 单卡：
  - `8px` 圆角
  - `1px` 边框
  - hover 阴影增强
- 顶角状态 badge 覆盖在卡片右上角
- 底部操作区单独有浅灰背景和分割线

详情查看：

- 使用右侧抽屉，不跳整页
- 模板抽屉宽 `520px`

## 8. 详情页

搜索锚点：

- `detail-section-card`
- `detail-route-panel`
- `detail-pagination`

详情页是**多段卡片堆叠**，不是单卡信息墙。

标准组合：

1. 产品信息区
2. 收发件人双卡区
3. 物流路由区
4. 关联表格区

固定规则：

- section card padding `20px 24px`
- section title `18px / 500`
- 产品区顶部可带操作按钮组
- 描述信息使用 `Descriptions`
- 路由区使用浅灰承载面板 + 步骤条
- 关联信息仍然用 `VxeTable`
- 分页在最下方右对齐

## 9. 结果页与异常页

搜索锚点：

- `result-page-card`

固定规则：

- 整页使用单白卡居中
- 内容核心为 `a-result`
- 保留默认按钮操作区
- 不额外叠加复杂筛选或侧信息

## 10. 看板页与视觉页

搜索锚点：

- `dashboard-page`
- `visual-dashboard-page`
- `data-screen`

固定规则：

- 常规看板复用统计卡、双栏图表区、底部图表区结构
- 强视觉页复用欢迎横幅、统计卡、排行榜、待办、勋章等更装饰化卡片
- 数据大屏直接复用现有大屏块，不要自行拼一套新的大屏视觉语言

## 11. 不确定时的选型

- 业务主流程简单录入：选 `basic-form`
- 多分组录入且夹带表格、抽屉、弹窗：选 `advanced-form`
- 有明确步骤推进：选 `step-form`
- 筛选 + 明细表：选 `query-table`
- 人员/资源/站点卡片概览：选 `card-list`
- 信息汇总、链路、路由、双侧信息：选 `detail`
- 运营概览：选 `dashboard-basic` 或 `dashboard-visual`
