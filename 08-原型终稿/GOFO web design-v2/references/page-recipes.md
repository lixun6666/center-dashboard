# Page Recipes

这份文件把真实 GOFO 管理后台里的典型页面模块，整理成可直接套用的“页面配方”。

## 使用方法

1. 先根据 PRD 选中最接近的配方。
2. 去 `assets/template/gofo-admin.html` 搜对应锚点。
3. 复制整段页面块及其相邻 CSS / state / handler，再改业务内容。
4. 如果找不到 100% 一致的页面，就用最接近的配方组合。

## 1. 固定框架空页

适用：

- 只需要 GOFO 的顶栏 + 侧栏 + tabs 骨架
- 先搭空壳，后续再填内容

模板 key：

- `foundation`

锚点：

- `activeTab === 'foundation'`

处理方式：

- 保留固定骨架
- `.gofo-content` 可以暂时只放空占位

## 2. 查询表格页

适用：

- 查询条件 + 明细数据表
- 运营规则、订单、任务、组织、司机、站点等管理页

模板 key：

- `query-table`

锚点：

- `activeTab === 'query-table'`
- `query-filter-card`
- `query-table-toolbar`

页面结构：

1. 顶部筛选卡
2. 列表卡
3. 工具栏
4. `VxeTable`
5. 分页

优先复用：

- 四列筛选栅格
- 展开/收起附加筛选
- 工具栏左右结构
- 图标操作列
- 列设置弹层

当 PRD 是“列表 + 批量操作 + 导出导入”时，优先用它。

## 3. 卡片列表页

适用：

- 人员卡片
- 站点卡片
- 路区卡片
- 资源池卡片

模板 key：

- `card-list`

锚点：

- `activeTab === 'card-list'`
- `card-list-grid`

页面结构：

1. 顶部筛选卡
2. 卡片网格
3. 底部分页
4. 右侧详情抽屉

优先复用：

- 右上状态 badge
- 头像区
- 关键信息行
- 进度条
- 卡片底部操作带

## 4. 基础表单页

适用：

- 新建规则
- 基础信息录入
- 单阶段申请/编辑页

模板 key：

- `basic-form`

锚点：

- `activeTab === 'basic-form'`
- `basic-form-card`

页面结构：

1. 单卡片
2. 横向表单
3. 底部操作

优先复用：

- 标题/日期/描述/输入/公开范围 这种基础字段节奏
- `320px` 输入宽与 `480px` 文本域宽
- `重置 + 提交` 尾部操作

## 5. 高级表单页

适用：

- 复杂业务录入
- 多分组表单
- 同页出现内嵌表格、详情弹窗、编辑抽屉

模板 key：

- `advanced-form`

锚点：

- `activeTab === 'advanced-form'`
- `advanced-form-section-card`

页面结构：

1. 分组 section card
2. 垂直表单栅格
3. 可展开补充字段
4. 管理表格
5. 详情弹窗
6. 编辑抽屉
7. 底部操作条

当 PRD 同时出现“表单 + 子表 + 行操作 + 弹窗/抽屉”时，优先选它。

## 6. 分步表单页

适用：

- 支付流程
- 审批流
- 开通/创建向导
- 需要“填写 -> 确认 -> 完成”的业务

模板 key：

- `step-form`

锚点：

- `activeTab === 'step-form'`
- `step-form-steps`

页面结构：

1. 步骤条
2. 编辑态表单
3. 确认态信息卡
4. 完成态结果

当 PRD 明确有步骤推进、回看确认或成功页闭环时，优先选它。

## 7. 详情页

适用：

- 运单详情
- 订单详情
- 档案详情
- 路由链路详情
- 复合信息概览页

模板 key：

- `detail`

锚点：

- `activeTab === 'detail'`
- `detail-section-card`

页面结构：

1. 顶部产品与数据概览
2. 成对信息卡
3. 路由或流程面板
4. 关联表格

当 PRD 同时包含“关键信息 + 结构化描述 + 子对象 + 路径/状态链路 + 相关列表”时，优先选它。

## 8. 成功/失败/异常页

适用：

- 流程完成反馈
- 权限异常
- 页面不存在
- 服务错误

模板 key：

- `success`
- `fail`
- `403`
- `404`
- `500`

锚点：

- `result-page-card`

处理方式：

- 用模板里的 `a-result`
- 保持白卡居中
- 只替换标题、副标题、按钮文案

## 9. 常规数据看板

适用：

- 业务总览
- 运营看板
- 指标监控首页

模板 key：

- `dashboard-basic`

锚点：

- `activeTab === 'dashboard-basic'`
- `dashboard-stat-row`
- `dashboard-middle-row`

页面结构：

1. 顶部统计卡
2. 中间双栏卡片
3. 底部图表区

优先复用：

- 30px 大数字
- 迷你趋势图
- 饼图图例
- 底部线图

## 10. 复合监控看板

适用：

- 实时看板
- 多 tab 监控页
- 站长 / 运营盯盘页
- PRD 明确给出多个业务 tab，且每个 tab 模块结构明显不同的页面

模板 key：

- `dashboard-monitoring`

锚点：

- `query-filter-card`
- `activeTab === 'dashboard-basic'`
- `detail-section-card`
- `query-table-toolbar`

页面结构：

1. 顶部筛选卡
2. 页面内 line tabs（顺序与文案严格按 PRD）
3. 每个 tab 独立模块区
4. 模块内 workbench / 列表 / 图表 / 弹窗

拼装规则：

- 真实模板没有 100% 对应整页时，允许新增业务 `activeTab`，但必须用既有配方组合，不得通过删模块来“适配模板”
- tab 已承担主标题时，内容区默认不重复渲染同名标题，除非 PRD 明确要求
- 概览、库内、派送、考核这类 tab 视为不同布局系统，不要把同一套栅格机械复用到所有 tab
- 需要“筛选 + 表格”的模块，直接继承 `query-table`
- 需要“左侧列表 / 右侧详情”的模块，优先继承 `detail` 或 `card-list` 的分栏模式
- 信息密度高时，优先做清晰分组和稳定栅格，不要留下无法承载业务含义的大面积空白

交付前至少回答：

- PRD 里的每个 tab 是否保留
- 每个 tab 的模块是否逐项映射
- 哪些模块复用了哪一类配方

## 11. 强视觉工作台

适用：

- 管理驾驶舱
- 站长/队长首页
- 希望更强品牌氛围的运营首页

模板 key：

- `dashboard-visual`

锚点：

- `activeTab === 'dashboard-visual'`
- `visual-hero-banner`
- `visual-stat-row`

页面结构：

1. 欢迎横幅
2. 强视觉统计卡
3. 中部三栏内容
4. 排行、勋章、待办、司机状态等模块

只有当用户明确希望“更强视觉”时，才从这个模板演化。默认后台首页优先普通看板。

## 12. 数据大屏

适用：

- 大屏展示
- 指挥中心
- 长时间展示的监控页

模板 key：

- `data-screen`

锚点：

- `activeTab === 'data-screen'`

处理方式：

- 直接复用已有大屏页面结构和图表块
- 不要把普通后台页硬改成大屏风格

## 13. 新页面判型规则

当 PRD 没有完全对上现成页面时，按下面规则选：

- “管理列表 / 查询 / 批量操作 / 导出导入” -> `query-table`
- “资源卡片 / 人员卡片 / 设备卡片 / 站点卡片” -> `card-list`
- “简单创建 / 编辑 / 提交申请” -> `basic-form`
- “复杂录入 / 分组录入 / 子表操作” -> `advanced-form`
- “多步骤流程 / 向导” -> `step-form`
- “信息汇总 / 物流链路 / 审核详情 / 档案详情” -> `detail`
- “首页 / 总览 / 指标看板” -> `dashboard-basic`
- “多 Tab 监控 / 实时看板 / 站点盯盘 / 运营监控” -> `dashboard-monitoring`
- “驾驶舱 / 氛围感更强首页” -> `dashboard-visual`
- “大屏展示” -> `data-screen`
