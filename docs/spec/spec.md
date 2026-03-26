> **版本**: 1.0.0 | **状态**: draft | **更新时间**: 2026-03-26T00:00:00Z
>
> **来源方案**: normal 场景 1.0.0 版本

---

#### 约定说明

本文档使用以下标准值:
- **布局类型**: `list` / `detail` / `form` / `dashboard` / `steps` / `custom`
- **区块类型**: `table` / `form` / `card` / `cards` / `chart` / `tabs` / `steps` / `timeline` / `description` / `statistic` / `custom`
- **字段类型**: `text` / `textarea` / `number` / `money` / `date` / `datetime` / `select` / `multiselect` / `switch` / `upload` 等 (完整列表见下游约定)

---

## 一、产品概述

### 1.1 项目背景

宜昌长机科技做数控齿轮加工机床，去年刚拿下来 DCMM 认证。车间用西门子系统管生产，但设备维护还靠老师傅经验，预防性维护基本没有，导致关键工序质量不稳定。上 EAM 系统主要是为了解决三个问题：减少意外停机、提升维修效率、把维修经验沉淀下来。

### 1.2 产品目标

- 建立完整的设备全生命周期台账,实现设备信息统一管理和快速查询
- 实现维修工单全流程数字化管理,缩短响应时间50%以上
- 建立预防性维护计划体系,将预防性维护覆盖率提升至80%以上
- 沉淀维修知识库,支持典型故障案例归档和知识传承
- 实现备品备件透明化管理,提升库存周转率30%以上

### 1.3 目标用户

| 角色 | 描述 | 核心诉求 |
|------|------|----------|
| 设备管理员 | 负责设备台账管理、维护计划制定、报表分析 | 快速查询设备信息、制定维护计划、掌握设备管理现状 |
| 维修工程师 | 负责故障维修、预防性维护执行 | 快速接收工单、查询设备资料、记录维修过程 |
| 设备操作工 | 负责日常设备点检、故障报修 | 便捷报修、查看报修进度 |
| 采购仓储 | 负责备品备件采购和库存管理 | 实时掌握库存、及时补充备件、统计分析消耗 |

### 1.4 范围定义

**本期包含**
- 设备台账管理: 设备分类编码、技术参数档案、位置变动追踪、文档附件管理
- 工单管理: 移动端报修、工单状态跟踪、维修知识库沉淀、工时和成本记录
- 预防性维护: 按时间/计量值触发、维护模板复用、工单自动生成、执行进度跟踪
- 备品备件管理: 备件与设备关联、安全库存预警、消耗统计分析、采购申请
- 报表分析: 设备综合报表、工单分析报表、备件分析报表、自定义报表、数据可视化

**本期不含**
- 与西门子数控系统的实时数据集成 (待客户明确集成需求后再实施)
- 设备物联网传感器接入 (非核心需求)
- 移动端原生App (采用响应式Web替代)
- 设备租赁/调拨管理 (非业务场景)
- 多工厂/多组织管理 (单一工厂场景)

---

## 二、信息架构

### 2.1 站点地图

```
EAM系统
├── 首页 (Dashboard)                     /dashboard            [LayoutDashboard]
├── 设备台账
│   ├── 设备列表                         /equipment            [LayoutList]
│   ├── 设备详情                         /equipment/:id        [LayoutDetail]
│   └── 新建设备                         /equipment/new        [LayoutForm]
├── 工单管理
│   ├── 工单列表                         /work-orders          [LayoutList]
│   ├── 工单详情                         /work-orders/:id      [LayoutDetail]
│   ├── 创建工单                         /work-orders/new      [LayoutForm]
│   └── 我的工单                         /work-orders/my       [LayoutList]
├── 预防性维护
│   ├── 维护计划                         /maintenance/plans    [LayoutList]
│   ├── 计划详情                         /maintenance/plans/:id [LayoutDetail]
│   ├── 创建计划                         /maintenance/plans/new [LayoutForm]
│   └── 维护日历                         /maintenance/calendar [LayoutCustom]
├── 备品备件
│   ├── 备件列表                         /spare-parts          [LayoutList]
│   ├── 备件详情                         /spare-parts/:id      [LayoutDetail]
│   ├── 新增备件                         /spare-parts/new      [LayoutForm]
│   └── 库存预警                         /spare-parts/alerts   [LayoutList]
├── 报表分析
│   ├── 设备综合报表                     /reports/equipment    [LayoutCustom]
│   ├── 工单分析报表                     /reports/work-orders  [LayoutCustom]
│   ├── 备件分析报表                     /reports/spare-parts  [LayoutCustom]
│   └── 自定义报表                       /reports/custom       [LayoutCustom]
└── 系统设置
    ├── 用户管理                         /settings/users       [LayoutList]
    ├── 角色权限                         /settings/roles       [LayoutList]
    └── 数据字典                         /settings/dictionaries [LayoutList]
```

### 2.2 导航结构

| 一级菜单 | 二级菜单 | 路由 | 说明 |
|----------|----------|------|------|
| 首页 | - | /dashboard | 设备管理数据概览 |
| 设备台账 | 设备列表 | /equipment | 查看和管理所有设备 |
| 设备台账 | 新建设备 | /equipment/new | 录入新设备信息 |
| 工单管理 | 工单列表 | /work-orders | 查看和管理所有工单 |
| 工单管理 | 创建工单 | /work-orders/new | 手动创建工单 |
| 工单管理 | 我的工单 | /work-orders/my | 当前用户的工单 |
| 预防性维护 | 维护计划 | /maintenance/plans | 查看和管理维护计划 |
| 预防性维护 | 维护日历 | /maintenance/calendar | 日历视图查看维护任务 |
| 预防性维护 | 创建计划 | /maintenance/plans/new | 创建新的维护计划 |
| 备品备件 | 备件列表 | /spare-parts | 查看和管理所有备件 |
| 备品备件 | 库存预警 | /spare-parts/alerts | 查看低库存预警 |
| 备品备件 | 新增备件 | /spare-parts/new | 录入新备件信息 |
| 报表分析 | 设备综合报表 | /reports/equipment | 设备台账、状态、维护记录 |
| 报表分析 | 工单分析报表 | /reports/work-orders | 工单数量、响应时间、完成率 |
| 报表分析 | 备件分析报表 | /reports/spare-parts | 库存周转、消耗趋势、成本统计 |
| 报表分析 | 自定义报表 | /reports/custom | 用户自定义报表 |
| 系统设置 | 用户管理 | /settings/users | 管理系统用户 |
| 系统设置 | 角色权限 | /settings/roles | 管理角色和权限 |
| 系统设置 | 数据字典 | /settings/dictionaries | 管理枚举值字典 |

---

## 三、功能模块

### 3.1 首页 (Dashboard)

> 为管理层和设备管理员提供设备管理现状的实时数据概览,支持快速掌握关键指标和待办事项。

#### 3.1.1 首页概览

**路由**: `/dashboard`
**布局**: `dashboard`
**描述**: 设备管理数据概览仪表板

##### 统计卡片 (statistic)

| 指标 | 说明 |
|------|------|
| 设备总数 | 统计所有在册设备数量 |
| 运行中设备 | 统计当前状态为运行的设备数量 |
| 待处理工单 | 统计状态为待处理和处理中的工单数量 |
| 今日完成工单 | 统计今日已完成的工单数量 |
| 低库存备件 | 统计库存低于最小库存的备件数量 |
| 本月预防性维护 | 统计本月计划执行的预防性维护任务数量 |

##### 工单趋势图表 (chart)

- 图表类型: 折线图
- 维度: 最近7天/30天
- 指标: 工单数量、完成数量

##### 待办事项列表 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 类型 | type | tag | 否 | 工单/维护任务 |
| 内容 | content | text | 否 | 待办事项描述 |
| 截止时间 | deadline | date | 是 | 任务截止日期 |
| 优先级 | priority | tag | 否 | 高/中/低 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 查看全部工单 | primary | toolbar-right | navigate → /work-orders |
| 查看全部任务 | default | toolbar-right | navigate → /maintenance/plans |

##### 业务规则

- 统计数据实时计算,每日凌晨更新缓存
- 待办事项按截止时间升序排列,逾期任务标红
- 低库存备件仅显示库存低于最小库存的备件

---

### 3.2 设备台账管理

> 建立完整的设备全生命周期台账,实现设备信息统一管理,支持快速查询设备状态和历史记录,为预防性维护计划和数据分析提供基础。

#### 3.2.1 设备列表

**路由**: `/equipment`
**布局**: `list`
**描述**: 查看和管理所有设备,支持多维度筛选和查询

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 设备名称 | name | text | 否 | 模糊搜索 |
| 设备编码 | code | text | 否 | 精确匹配 |
| 设备分类 | categoryId | select | 否 | 选项来源: dict-equipment-category |
| 设备状态 | status | select | 否 | 选项来源: dict-equipment-status |
| 位置 | locationId | select | 否 | 选项来源: dict-location |

##### 设备列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 设备编码 | code | text | 是 | 唯一标识 |
| 设备名称 | name | text | 是 | 设备名称 |
| 设备分类 | categoryName | tag | 是 | 显示分类名称 |
| 型号规格 | model | text | 是 | 设备型号 |
| 位置 | locationName | text | 是 | 设备位置 |
| 设备状态 | status | status | 是 | 运行/停机/维修/报废 |
| 购置日期 | purchaseDate | date | 是 | 设备购置日期 |
| 操作 | - | action | 否 | 查看/编辑/删除 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 新建设备 | primary | toolbar-right | navigate → /equipment/new |
| 批量导入 | default | toolbar-right | modal → 导入设备数据 |
| 导出 | default | toolbar-right | download → 导出设备列表 |
| 查看 | text | row | navigate → /equipment/:id |
| 编辑 | text | row | navigate → /equipment/:id/edit |
| 删除 | text | row | action → 删除确认弹窗 |

##### 业务规则

- 设备编码全局唯一,重复时提示
- 删除设备前检查是否有关联工单,有则禁止删除
- 导出支持Excel格式,包含表格所有列

#### 3.2.2 设备详情

**路由**: `/equipment/:id`
**布局**: `detail`
**描述**: 查看设备详细信息、维修历史、关联备件

##### 基本信息区块 (description)

| 字段 | fieldKey | 显示 |
|------|----------|------|
| 设备编码 | code | 文本 |
| 设备名称 | name | 文本 |
| 设备分类 | categoryName | 标签 |
| 型号规格 | model | 文本 |
| 制造商 | manufacturer | 文本 |
| 出厂日期 | manufactureDate | 日期 |
| 购置日期 | purchaseDate | 日期 |
| 位置 | locationName | 文本 |
| 设备状态 | status | 状态标签 |
| 负责人 | responsibleUserName | 文本 |
| 技术参数 | technicalParams | 富文本 |

##### 标签页 (tabs)

**维修历史** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 工单编号 | workOrderNo | link | 是 | 跳转至工单详情 |
| 故障描述 | description | text | 否 | 故障简要描述 |
| 维修类型 | type | tag | 否 | 纠正性/预防性 |
| 开始时间 | startTime | datetime | 是 | 维修开始时间 |
| 完成时间 | endTime | datetime | 是 | 维修完成时间 |
| 维修人 | repairUserName | text | 否 | 维修人员 |

**关联备件** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 备件编码 | sparePartCode | link | 是 | 跳转至备件详情 |
| 备件名称 | sparePartName | text | 是 | 备件名称 |
| 规格型号 | specification | text | 否 | 规格型号 |
| 安全库存 | safetyStock | number | 是 | 最小库存量 |
| 当前库存 | currentStock | number | 是 | 当前库存量 |

**附件文档** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 文档名称 | name | text | 否 | 文档名称 |
| 文档类型 | type | tag | 否 | 说明书/图纸/保养手册 |
| 上传时间 | uploadTime | datetime | 是 | 上传时间 |
| 上传人 | uploaderName | text | 否 | 上传人 |
| 操作 | - | action | 否 | 下载/删除 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 编辑 | primary | toolbar-right | navigate → /equipment/:id/edit |
| 创建工单 | primary | toolbar-right | navigate → /work-orders/new?equipmentId=:id |
| 删除设备 | danger | toolbar-right | action → 删除确认弹窗 |
| 上传附件 | default | card-header | modal → 上传文档附件 |
| 下载 | text | row | download → 下载附件 |

##### 业务规则

- 维修历史按完成时间倒序排列
- 关联备件仅显示该设备专用的备件
- 附件支持pdf、doc、docx、jpg、png格式,单文件不超过10MB

#### 3.2.3 新建设备/编辑设备

**路由**: `/equipment/new` `/equipment/:id/edit`
**布局**: `form`
**描述**: 录入新设备信息或编辑现有设备

##### 基本信息表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 设备编码 | code | text | 是 | 全局唯一,建议格式: SB + 年月 + 序号 |
| 设备名称 | name | text | 是 | 设备名称 |
| 设备分类 | categoryId | select | 是 | 选项来源: dict-equipment-category |
| 型号规格 | model | text | 否 | 设备型号规格 |
| 制造商 | manufacturer | text | 否 | 设备制造商 |
| 出厂日期 | manufactureDate | date | 否 | 设备出厂日期 |
| 购置日期 | purchaseDate | date | 否 | 设备购置日期 |
| 位置 | locationId | select | 否 | 选项来源: dict-location |
| 设备状态 | status | select | 是 | 选项来源: dict-equipment-status,默认: 运行 |
| 负责人 | responsibleUserId | user | 否 | 设备负责人 |
| 技术参数 | technicalParams | richtext | 否 | 设备技术参数详情 |
| 备注 | remark | textarea | 否 | 备注信息 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 保存 | primary | form-footer | action → 保存并返回列表 |
| 保存并继续 | default | form-footer | action → 保存并继续编辑 |
| 取消 | default | form-footer | navigate → /equipment |

##### 业务规则

- 设备编码保存时校验唯一性
- 设备分类为必选项,不可为空
- 编辑时设备编码不可修改

---

### 3.3 工单管理

> 实现故障报修、派工、执行、验收、归档的闭环管理,支持移动端报修,缩短响应时间,沉淀维修知识库。

#### 3.3.1 工单列表

**路由**: `/work-orders`
**布局**: `list`
**描述**: 查看和管理所有工单,支持按状态、优先级、时间筛选

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 工单编号 | workOrderNo | text | 否 | 精确匹配 |
| 设备名称 | equipmentName | text | 否 | 模糊搜索 |
| 工单状态 | status | select | 否 | 选项来源: dict-work-order-status |
| 优先级 | priority | select | 否 | 选项来源: dict-priority |
| 维修类型 | type | select | 否 | 选项来源: dict-work-order-type |
| 报修时间 | reportTimeRange | daterange | 否 | 报修时间范围 |

##### 工单列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 工单编号 | workOrderNo | link | 是 | 跳转至工单详情 |
| 设备名称 | equipmentName | text | 否 | 报修设备名称 |
| 故障描述 | description | text | 否 | 故障简要描述 |
| 工单状态 | status | status | 是 | 待处理/处理中/待验收/已完成/已关闭 |
| 优先级 | priority | tag | 是 | 高/中/低 |
| 维修类型 | type | tag | 否 | 纠正性/预防性 |
| 报修人 | reportUserName | text | 否 | 报修人 |
| 报修时间 | reportTime | datetime | 是 | 报修时间 |
| 操作 | - | action | 否 | 查看/派工/关闭 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 创建工单 | primary | toolbar-right | navigate → /work-orders/new |
| 批量派工 | default | toolbar-right | action → 批量派工弹窗 |
| 导出 | default | toolbar-right | download → 导出工单列表 |
| 查看 | text | row | navigate → /work-orders/:id |
| 派工 | text | row | modal → 派工弹窗 (仅待处理状态) |
| 关闭 | text | row | action → 关闭确认 (仅处理中状态) |

##### 业务规则

- 工单编号自动生成,格式: GD + 年月日 + 序号
- 列表默认按报修时间倒序排列
- 批量派工仅支持待处理状态的工单

#### 3.3.2 工单详情

**路由**: `/work-orders/:id`
**布局**: `detail`
**描述**: 查看工单详细信息、处理进度、维修记录

##### 工单信息区块 (description)

| 字段 | fieldKey | 显示 |
|------|----------|------|
| 工单编号 | workOrderNo | 文本 |
| 设备名称 | equipmentName | 链接 (跳转至设备详情) |
| 故障描述 | description | 文本 |
| 工单状态 | status | 状态标签 |
| 优先级 | priority | 标签 |
| 维修类型 | type | 标签 |
| 报修人 | reportUserName | 文本 |
| 报修时间 | reportTime | 日期时间 |
| 维修人 | repairUserName | 文本 |
| 开始时间 | startTime | 日期时间 |
| 完成时间 | completeTime | 日期时间 |

##### 标签页 (tabs)

**处理进度** (timeline)

| 字段 | fieldKey | 显示 |
|------|----------|------|
| 操作时间 | operateTime | 日期时间 |
| 操作人 | operatorName | 文本 |
| 操作内容 | content | 文本 |
| 备注 | remark | 文本 |

**维修记录** (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 故障现象 | phenomenon | textarea | 是 | 故障现象描述 |
| 故障原因 | cause | textarea | 否 | 故障原因分析 |
| 处理方法 | solution | textarea | 是 | 处理方法记录 |
| 更换备件 | spareParts | textarea | 否 | 更换的备件清单 |
| 工时 | workHours | number | 否 | 维修工时 (小时) |
| 维修照片 | photos | upload | 否 | 支持多张图片 |

**备件消耗** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 备件编码 | sparePartCode | link | 是 | 跳转至备件详情 |
| 备件名称 | sparePartName | text | 是 | 备件名称 |
| 规格型号 | specification | text | 否 | 规格型号 |
| 消耗数量 | quantity | number | 是 | 消耗数量 |
| 单位 | unit | text | 否 | 单位 |
| 单价 | unitPrice | money | 是 | 单价 |
| 金额 | amount | money | 是 | 金额 = 数量 × 单价 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 编辑 | primary | toolbar-right | navigate → /work-orders/:id/edit (仅待处理/处理中) |
| 开始处理 | primary | toolbar-right | action → 开始维修 (仅待处理状态) |
| 完成维修 | primary | toolbar-right | action → 完成维修 (仅处理中状态) |
| 关闭工单 | default | toolbar-right | action → 关闭确认 (仅已完成状态) |
| 添加备件消耗 | default | card-header | modal → 添加备件消耗 |
| 保存维修记录 | primary | card-footer | action → 保存维修记录 |

##### 业务规则

- 仅维修人可编辑维修记录
- 完成维修时必填维修记录
- 工单关闭后不可再编辑
- 备件消耗自动扣减库存

#### 3.3.3 创建工单

**路由**: `/work-orders/new`
**布局**: `form`
**描述**: 手动创建维修工单

##### 工单信息表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 设备 | equipmentId | select | 是 | 选项来源: 设备列表 |
| 故障描述 | description | textarea | 是 | 故障详细描述 |
| 优先级 | priority | select | 是 | 选项来源: dict-priority,默认: 中 |
| 维修类型 | type | select | 是 | 选项来源: dict-work-order-type |
| 报修人 | reportUserId | user | 是 | 默认当前用户 |
| 报修时间 | reportTime | datetime | 是 | 默认当前时间 |
| 故障照片 | photos | upload | 否 | 支持多张图片 |
| 备注 | remark | textarea | 否 | 备注信息 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 提交 | primary | form-footer | action → 提交并返回列表 |
| 保存草稿 | default | form-footer | action → 保存为草稿 |
| 取消 | default | form-footer | navigate → /work-orders |

##### 业务规则

- 工单创建后状态默认为"待处理"
- 故障描述为必填项,至少10个字符
- 故障照片支持jpg、png格式,单张不超过5MB

#### 3.3.4 我的工单

**路由**: `/work-orders/my`
**布局**: `list`
**描述**: 查看当前用户的工单 (包括报修的工单和被派工的工单)

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 工单状态 | status | select | 否 | 选项来源: dict-work-order-status |
| 优先级 | priority | select | 否 | 选项来源: dict-priority |
| 角色 | role | select | 否 | 报修人/维修人 |

##### 工单列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 工单编号 | workOrderNo | link | 是 | 跳转至工单详情 |
| 设备名称 | equipmentName | text | 否 | 报修设备名称 |
| 故障描述 | description | text | 否 | 故障简要描述 |
| 工单状态 | status | status | 是 | 待处理/处理中/待验收/已完成/已关闭 |
| 优先级 | priority | tag | 是 | 高/中/低 |
| 角色 | role | tag | 否 | 报修人/维修人 |
| 报修时间 | reportTime | datetime | 是 | 报修时间 |
| 操作 | - | action | 否 | 查看/接单/开始/完成 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 刷新 | default | toolbar-right | action → 刷新列表 |
| 查看 | text | row | navigate → /work-orders/:id |
| 接单 | text | row | action → 接单 (仅待处理且角色为维修人) |
| 开始处理 | text | row | action → 开始维修 (仅处理中且角色为维修人) |
| 完成维修 | text | row | action → 完成维修 (仅处理中且角色为维修人) |

##### 业务规则

- 列表显示当前用户作为报修人或维修人的所有工单
- 默认按报修时间倒序排列

---

### 3.4 预防性维护

> 基于设备类型和运行时长自动生成预防性维护计划,支持按时间周期或计量值触发,自动生成工单并提醒,提升预防性维护覆盖率。

#### 3.4.1 维护计划列表

**路由**: `/maintenance/plans`
**布局**: `list`
**描述**: 查看和管理所有预防性维护计划

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 计划名称 | name | text | 否 | 模糊搜索 |
| 设备分类 | categoryId | select | 否 | 选项来源: dict-equipment-category |
| 触发方式 | triggerType | select | 否 | 选项来源: dict-trigger-type |
| 计划状态 | status | select | 否 | 选项来源: dict-plan-status |

##### 维护计划列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 计划名称 | name | text | 是 | 维护计划名称 |
| 设备分类 | categoryName | tag | 是 | 适用设备分类 |
| 维护类型 | maintenanceType | tag | 否 | 日保/周保/月保/年保 |
| 触发方式 | triggerType | tag | 是 | 时间/计量值 |
| 触发周期 | triggerCycle | text | 是 | 如: 每3个月/每500小时 |
| 计划状态 | status | status | 是 | 启用/停用 |
| 下次执行时间 | nextExecuteTime | datetime | 是 | 下次计划执行时间 |
| 操作 | - | action | 否 | 查看/编辑/停用/启用 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 创建计划 | primary | toolbar-right | navigate → /maintenance/plans/new |
| 批量启用 | default | toolbar-right | action → 批量启用 |
| 批量停用 | default | toolbar-right | action → 批量停用 |
| 导出 | default | toolbar-right | download → 导出计划列表 |
| 查看 | text | row | navigate → /maintenance/plans/:id |
| 编辑 | text | row | navigate → /maintenance/plans/:id/edit |
| 停用 | text | row | action → 停用确认 (仅启用状态) |
| 启用 | text | row | action → 启用确认 (仅停用状态) |

##### 业务规则

- 列表默认按下次执行时间升序排列
- 停用的计划不会生成工单

#### 3.4.2 计划详情

**路由**: `/maintenance/plans/:id`
**布局**: `detail`
**描述**: 查看维护计划详细信息、适用设备、执行历史

##### 计划信息区块 (description)

| 字段 | fieldKey | 显示 |
|------|----------|------|
| 计划名称 | name | 文本 |
| 设备分类 | categoryName | 标签 |
| 维护类型 | maintenanceType | 标签 |
| 触发方式 | triggerType | 标签 |
| 触发周期 | triggerCycle | 文本 |
| 计划状态 | status | 状态标签 |
| 提前提醒天数 | advanceDays | 文本 |
| 维护内容 | maintenanceContent | 富文本 |
| 创建时间 | createTime | 日期时间 |
| 创建人 | creatorName | 文本 |

##### 标签页 (tabs)

**适用设备** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 设备编码 | code | link | 是 | 跳转至设备详情 |
| 设备名称 | name | text | 是 | 设备名称 |
| 型号规格 | model | text | 否 | 型号规格 |
| 位置 | locationName | text | 否 | 设备位置 |
| 上次执行时间 | lastExecuteTime | datetime | 是 | 上次维护执行时间 |
| 下次执行时间 | nextExecuteTime | datetime | 是 | 下次计划执行时间 |

**执行历史** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 工单编号 | workOrderNo | link | 是 | 跳转至工单详情 |
| 设备名称 | equipmentName | text | 是 | 设备名称 |
| 计划执行时间 | planTime | datetime | 是 | 计划执行时间 |
| 实际执行时间 | actualTime | datetime | 是 | 实际执行时间 |
| 执行人 | executorName | text | 否 | 执行人 |
| 执行状态 | status | status | 是 | 已完成/已跳过/已逾期 |

**维护标准** (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 维护项目 | maintenanceItems | textarea | 是 | 维护项目清单,一行一项 |
| 所需备件 | spareParts | textarea | 否 | 所需备件清单 |
| 所需工具 | tools | textarea | 否 | 所需工具清单 |
| 安全注意事项 | safetyNotes | textarea | 否 | 安全注意事项 |
| 参考工时 | referenceHours | number | 否 | 参考工时 (小时) |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 编辑 | primary | toolbar-right | navigate → /maintenance/plans/:id/edit |
| 停用 | default | toolbar-right | action → 停用确认 (仅启用状态) |
| 启用 | default | toolbar-right | action → 启用确认 (仅停用状态) |
| 添加设备 | default | card-header | modal → 选择设备弹窗 |
| 移除设备 | text | row | action → 移除确认 |

##### 业务规则

- 适用设备列表仅显示该设备分类下的设备
- 移除设备不影响已生成的工单
- 执行历史按计划执行时间倒序排列

#### 3.4.3 创建计划

**路由**: `/maintenance/plans/new`
**布局**: `form`
**描述**: 创建新的预防性维护计划

##### 计划信息表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 计划名称 | name | text | 是 | 维护计划名称 |
| 设备分类 | categoryId | select | 是 | 选项来源: dict-equipment-category |
| 维护类型 | maintenanceType | select | 是 | 选项来源: dict-maintenance-type |
| 触发方式 | triggerType | select | 是 | 选项来源: dict-trigger-type |
| 触发周期 | triggerCycle | text | 是 | 如: 每3个月/每500小时 |
| 提前提醒天数 | advanceDays | number | 否 | 默认: 3天 |
| 维护内容 | maintenanceContent | richtext | 是 | 维护内容说明 |
| 维护项目 | maintenanceItems | textarea | 是 | 维护项目清单,一行一项 |
| 所需备件 | spareParts | textarea | 否 | 所需备件清单 |
| 所需工具 | tools | textarea | 否 | 所需工具清单 |
| 安全注意事项 | safetyNotes | textarea | 否 | 安全注意事项 |
| 参考工时 | referenceHours | number | 否 | 参考工时 (小时) |
| 计划状态 | status | switch | 是 | 默认: 启用 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 保存 | primary | form-footer | action → 保存并返回列表 |
| 保存并继续 | default | form-footer | action → 保存并继续编辑 |
| 取消 | default | form-footer | navigate → /maintenance/plans |

##### 业务规则

- 计划名称在同类设备分类下唯一
- 触发方式为"时间"时,触发周期格式为"每N天/周/月/年"
- 触发方式为"计量值"时,触发周期格式为"每N小时/次数"

#### 3.4.4 维护日历

**路由**: `/maintenance/calendar`
**布局**: `custom`
**描述**: 日历视图查看维护任务,支持按月查看

##### 日历视图 (custom)

- 日历类型: 月视图
- 显示内容: 计划名称、设备数量、执行状态
- 颜色标识: 正常(蓝色)、即将到期(橙色)、已逾期(红色)

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 设备分类 | categoryId | select | 否 | 选项来源: dict-equipment-category |
| 计划状态 | status | select | 否 | 选项来源: dict-plan-status |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 切换月份 | default | toolbar-left | action → 切换月份 |
| 切换视图 | default | toolbar-left | action → 月视图/周视图/日视图 |
| 刷新 | default | toolbar-right | action → 刷新日历 |

##### 业务规则

- 日历显示当前月份及前后各3天的任务
- 点击日期可查看当天所有维护任务
- 点击任务可跳转至对应工单或计划

---

### 3.5 备品备件管理

> 实现备品备件透明化管理,支持备件与设备关联、安全库存预警、消耗统计分析、采购申请,提升库存周转率,降低备件积压与短缺风险。

#### 3.5.1 备件列表

**路由**: `/spare-parts`
**布局**: `list`
**描述**: 查看和管理所有备件,支持多维度筛选和查询

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 备件名称 | name | text | 否 | 模糊搜索 |
| 备件编码 | code | text | 否 | 精确匹配 |
| 备件分类 | categoryId | select | 否 | 选项来源: dict-spare-part-category |
| 库存状态 | stockStatus | select | 否 | 选项来源: dict-stock-status |

##### 备件列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 备件编码 | code | text | 是 | 唯一标识 |
| 备件名称 | name | text | 是 | 备件名称 |
| 规格型号 | specification | text | 否 | 规格型号 |
| 单位 | unit | text | 否 | 单位 |
| 安全库存 | safetyStock | number | 是 | 最小库存量 |
| 当前库存 | currentStock | number | 是 | 当前库存量 |
| 库存状态 | stockStatus | status | 是 | 正常/低库存/缺货 |
| 单价 | unitPrice | money | 是 | 单价 |
| 供应商 | supplier | text | 否 | 供应商 |
| 操作 | - | action | 否 | 查看/编辑/入库/出库 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 新增备件 | primary | toolbar-right | navigate → /spare-parts/new |
| 批量导入 | default | toolbar-right | modal → 导入备件数据 |
| 导出 | default | toolbar-right | download → 导出备件列表 |
| 查看 | text | row | navigate → /spare-parts/:id |
| 编辑 | text | row | navigate → /spare-parts/:id/edit |
| 入库 | text | row | modal → 入库弹窗 |
| 出库 | text | row | modal → 出库弹窗 |

##### 业务规则

- 备件编码全局唯一,重复时提示
- 当前库存低于安全库存时,库存状态显示为"低库存"
- 当前库存为0时,库存状态显示为"缺货"

#### 3.5.2 备件详情

**路由**: `/spare-parts/:id`
**布局**: `detail`
**描述**: 查看备件详细信息、库存变动记录、关联设备

##### 基本信息区块 (description)

| 字段 | fieldKey | 显示 |
|------|----------|------|
| 备件编码 | code | 文本 |
| 备件名称 | name | 文本 |
| 备件分类 | categoryName | 标签 |
| 规格型号 | specification | 文本 |
| 单位 | unit | 文本 |
| 安全库存 | safetyStock | 文本 |
| 当前库存 | currentStock | 文本 |
| 库存状态 | stockStatus | 状态标签 |
| 单价 | unitPrice | 金额 |
| 供应商 | supplier | 文本 |
| 供应商联系方式 | supplierContact | 文本 |
| 备注 | remark | 文本 |

##### 标签页 (tabs)

**库存变动记录** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 变动时间 | changeTime | datetime | 是 | 库存变动时间 |
| 变动类型 | changeType | tag | 否 | 入库/出库/消耗 |
| 变动数量 | quantity | number | 是 | 变动数量 (正数入库,负数出库) |
| 变动后库存 | stockAfter | number | 是 | 变动后库存 |
| 关联工单 | workOrderNo | link | 否 | 跳转至工单详情 |
| 经手人 | operatorName | text | 否 | 经手人 |
| 备注 | remark | text | 否 | 备注 |

**关联设备** (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 设备编码 | equipmentCode | link | 是 | 跳转至设备详情 |
| 设备名称 | equipmentName | text | 是 | 设备名称 |
| 型号规格 | model | text | 否 | 型号规格 |
| 位置 | locationName | text | 否 | 设备位置 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 编辑 | primary | toolbar-right | navigate → /spare-parts/:id/edit |
| 入库 | primary | toolbar-right | modal → 入库弹窗 |
| 出库 | default | toolbar-right | modal → 出库弹窗 |
| 创建采购申请 | default | toolbar-right | navigate → /purchase-requests/new?sparePartId=:id |

##### 业务规则

- 库存变动记录按变动时间倒序排列
- 仅消耗类型的变动会关联工单

#### 3.5.3 新增备件/编辑备件

**路由**: `/spare-parts/new` `/spare-parts/:id/edit`
**布局**: `form`
**描述**: 录入新备件信息或编辑现有备件

##### 基本信息表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 备件编码 | code | text | 是 | 全局唯一,建议格式: BJ + 年月 + 序号 |
| 备件名称 | name | text | 是 | 备件名称 |
| 备件分类 | categoryId | select | 是 | 选项来源: dict-spare-part-category |
| 规格型号 | specification | text | 否 | 规格型号 |
| 单位 | unit | text | 否 | 选项来源: dict-unit |
| 安全库存 | safetyStock | number | 是 | 最小库存量 |
| 当前库存 | currentStock | number | 是 | 当前库存量,默认: 0 |
| 单价 | unitPrice | money | 是 | 单价 |
| 供应商 | supplier | text | 否 | 供应商名称 |
| 供应商联系方式 | supplierContact | text | 否 | 供应商联系方式 |
| 备注 | remark | textarea | 否 | 备注信息 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 保存 | primary | form-footer | action → 保存并返回列表 |
| 保存并继续 | default | form-footer | action → 保存并继续编辑 |
| 取消 | default | form-footer | navigate → /spare-parts |

##### 业务规则

- 备件编码保存时校验唯一性
- 当前库存不可为负数
- 安全库存必须大于0

#### 3.5.4 库存预警

**路由**: `/spare-parts/alerts`
**布局**: `list`
**描述**: 查看库存低于安全库存的备件预警

##### 预警列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 备件编码 | code | link | 是 | 跳转至备件详情 |
| 备件名称 | name | text | 是 | 备件名称 |
| 规格型号 | specification | text | 否 | 规格型号 |
| 当前库存 | currentStock | number | 是 | 当前库存量 |
| 安全库存 | safetyStock | number | 是 | 安全库存量 |
| 缺口数量 | gap | number | 是 | 缺口 = 安全库存 - 当前库存 |
| 供应商 | supplier | text | 否 | 供应商 |
| 操作 | - | action | 否 | 创建采购申请 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 批量创建采购申请 | primary | toolbar-right | action → 批量创建采购申请 |
| 导出 | default | toolbar-right | download → 导出预警列表 |
| 创建采购申请 | text | row | navigate → /purchase-requests/new?sparePartId=:id |

##### 业务规则

- 列表仅显示当前库存低于安全库存的备件
- 默认按缺口数量降序排列
- 缺口数量越大,优先级越高

---

### 3.6 报表分析

> 提供设备综合报表、工单分析报表、备件分析报表、自定义报表等多维度数据分析,支持数据可视化,为管理层提供决策支持。

#### 3.6.1 设备综合报表

**路由**: `/reports/equipment`
**布局**: `custom`
**描述**: 设备台账、运行状态、维护记录的综合分析报表

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 设备分类 | categoryId | select | 否 | 选项来源: dict-equipment-category |
| 位置 | locationId | select | 否 | 选项来源: dict-location |
| 时间范围 | dateRange | daterange | 否 | 报表时间范围 |

##### 统计卡片 (statistic)

| 指标 | 说明 |
|------|------|
| 设备总数 | 统计范围内的设备总数 |
| 运行设备数 | 统计状态为运行的设备数量 |
| 停机设备数 | 统计状态为停机的设备数量 |
| 维修设备数 | 统计状态为维修的设备数量 |
| 设备完好率 | 完好设备数 / 设备总数 × 100% |
| 平均设备年龄 | 设备年龄总和 / 设备数量 |

##### 图表区块 (chart)

**设备状态分布饼图**
- 维度: 设备状态
- 指标: 设备数量

**设备分类分布柱状图**
- 维度: 设备分类
- 指标: 设备数量

**设备位置分布柱状图**
- 维度: 位置
- 指标: 设备数量

**设备购置时间趋势折线图**
- 维度: 年份
- 指标: 设备数量

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 查询 | primary | toolbar-right | action → 刷新报表 |
| 导出 | default | toolbar-right | download → 导出Excel |
| 刷新 | default | toolbar-right | action → 刷新数据 |

##### 业务规则

- 报表数据每日凌晨更新缓存
- 导出Excel包含所有统计表格和图表数据

#### 3.6.2 工单分析报表

**路由**: `/reports/work-orders`
**布局**: `custom`
**描述**: 工单数量、响应时间、完成率、故障分布的综合分析报表

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 时间范围 | dateRange | daterange | 否 | 报表时间范围,默认: 本月 |
| 设备分类 | categoryId | select | 否 | 选项来源: dict-equipment-category |
| 工单状态 | status | multiselect | 否 | 选项来源: dict-work-order-status |

##### 统计卡片 (statistic)

| 指标 | 说明 |
|------|------|
| 工单总数 | 统计范围内的工单总数 |
| 已完成工单数 | 统计状态为已完成的工单数量 |
| 工单完成率 | 已完成工单数 / 工单总数 × 100% |
| 平均响应时间 | 从报修到开始处理平均时间 (分钟) |
| 平均处理时间 | 从开始处理到完成平均时间 (分钟) |
| 平均工时 | 平均维修工时 (小时) |

##### 图表区块 (chart)

**工单趋势折线图**
- 维度: 日期
- 指标: 工单数量、完成数量

**工单状态分布饼图**
- 维度: 工单状态
- 指标: 工单数量

**工单优先级分布饼图**
- 维度: 优先级
- 指标: 工单数量

**故障Top10设备柱状图**
- 维度: 设备名称
- 指标: 工单数量

**故障类型分布柱状图**
- 维度: 故障类型
- 指标: 工单数量

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 查询 | primary | toolbar-right | action → 刷新报表 |
| 导出 | default | toolbar-right | download → 导出Excel |
| 刷新 | default | toolbar-right | action → 刷新数据 |

##### 业务规则

- 平均响应时间仅计算已完成工单
- 平均处理时间仅计算已完成工单
- 报表数据每日凌晨更新缓存

#### 3.6.3 备件分析报表

**路由**: `/reports/spare-parts`
**布局**: `custom`
**描述**: 库存周转、消耗趋势、成本统计的综合分析报表

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 时间范围 | dateRange | daterange | 否 | 报表时间范围,默认: 本月 |
| 备件分类 | categoryId | select | 否 | 选项来源: dict-spare-part-category |

##### 统计卡片 (statistic)

| 指标 | 说明 |
|------|------|
| 备件总数 | 统计范围内的备件总数 |
| 库存总金额 | 备件库存总金额 |
| 低库存备件数 | 当前库存低于安全库存的备件数量 |
| 本月消耗金额 | 本月备件消耗总金额 |
| 库存周转率 | 本月消耗成本 / 平均库存成本 × 100% |
| 紧急采购次数 | 本月紧急采购次数 |

##### 图表区块 (chart)

**备件消耗趋势折线图**
- 维度: 日期
- 指标: 消耗金额

**备件消耗Top10柱状图**
- 维度: 备件名称
- 指标: 消耗金额

**备件分类消耗分布饼图**
- 维度: 备件分类
- 指标: 消耗金额

**库存周转率趋势折线图**
- 维度: 月份
- 指标: 库存周转率

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 查询 | primary | toolbar-right | action → 刷新报表 |
| 导出 | default | toolbar-right | download → 导出Excel |
| 刷新 | default | toolbar-right | action → 刷新数据 |

##### 业务规则

- 库存周转率按月计算
- 报表数据每日凌晨更新缓存

#### 3.6.4 自定义报表

**路由**: `/reports/custom`
**布局**: `custom`
**描述**: 用户自定义报表,支持自定义字段、筛选条件、导出格式

##### 报表模板列表 (cards)

| 字段 | fieldKey | 显示 |
|------|----------|------|
| 模板名称 | name | 文本 |
| 模板描述 | description | 文本 |
| 创建时间 | createTime | 日期 |
| 创建人 | creatorName | 文本 |
| 操作 | - | 操作按钮 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 新建报表 | primary | toolbar-right | navigate → /reports/custom/new |
| 使用模板 | text | card | navigate → /reports/custom/:id/edit |
| 编辑模板 | text | card | navigate → /reports/custom/:id/edit |
| 删除模板 | text | card | action → 删除确认 |

##### 业务规则

- 系统预置常用报表模板
- 用户可保存自定义报表为模板
- 模板仅创建人可编辑和删除

---

### 3.7 系统设置

> 管理系统用户、角色权限、数据字典等基础配置,确保系统安全性和可维护性。

#### 3.7.1 用户管理

**路由**: `/settings/users`
**布局**: `list`
**描述**: 管理系统用户,支持新增、编辑、禁用用户

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 用户名 | username | text | 否 | 模糊搜索 |
| 角色 | roleId | select | 否 | 选项来源: 角色列表 |
| 用户状态 | status | select | 否 | 选项来源: dict-user-status |

##### 用户列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 用户名 | username | text | 是 | 用户名 |
| 姓名 | name | text | 是 | 真实姓名 |
| 角色 | roleName | tag | 是 | 用户角色 |
| 部门 | department | text | 否 | 所属部门 |
| 手机号 | phone | text | 否 | 手机号 |
| 用户状态 | status | status | 是 | 启用/禁用 |
| 最后登录时间 | lastLoginTime | datetime | 是 | 最后登录时间 |
| 操作 | - | action | 否 | 查看/编辑/重置密码/禁用 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 新增用户 | primary | toolbar-right | navigate → /settings/users/new |
| 导出 | default | toolbar-right | download → 导出用户列表 |
| 查看 | text | row | navigate → /settings/users/:id |
| 编辑 | text | row | navigate → /settings/users/:id/edit |
| 重置密码 | text | row | action → 重置密码确认 |
| 禁用 | text | row | action → 禁用确认 (仅启用状态) |
| 启用 | text | row | action → 启用确认 (仅禁用状态) |

##### 业务规则

- 用户名全局唯一
- 禁用用户不可登录系统
- 重置密码后默认密码为123456

#### 3.7.2 角色权限

**路由**: `/settings/roles`
**布局**: `list`
**描述**: 管理系统角色和权限,支持新增、编辑角色及分配权限

##### 角色列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 角色名称 | name | text | 是 | 角色名称 |
| 角色描述 | description | text | 否 | 角色描述 |
| 用户数量 | userCount | number | 是 | 该角色下的用户数量 |
| 创建时间 | createTime | datetime | 是 | 创建时间 |
| 操作 | - | action | 否 | 查看/编辑/删除 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 新增角色 | primary | toolbar-right | navigate → /settings/roles/new |
| 查看 | text | row | navigate → /settings/roles/:id |
| 编辑 | text | row | navigate → /settings/roles/:id/edit |
| 删除 | text | row | action → 删除确认 (系统内置角色不可删除) |

##### 业务规则

- 系统内置角色不可删除
- 删除角色前检查是否有关联用户,有则禁止删除
- 权限按模块组织,支持细粒度控制

#### 3.7.3 数据字典

**路由**: `/settings/dictionaries`
**布局**: `list`
**描述**: 管理系统数据字典,支持新增、编辑字典项

##### 筛选表单 (form)

| 字段 | fieldKey | 类型 | 必填 | 说明 |
|------|----------|------|------|------|
| 字典名称 | name | text | 否 | 模糊搜索 |

##### 字典列表表格 (table)

| 列名 | fieldKey | 列类型 | 可排序 | 说明 |
|------|----------|--------|--------|------|
| 字典名称 | name | text | 是 | 字典名称 |
| 字典编码 | code | text | 是 | 字典编码 |
| 字典项数量 | itemCount | number | 是 | 字典项数量 |
| 说明 | description | text | 否 | 字典说明 |
| 操作 | - | action | 否 | 查看/编辑 |

##### 操作

| 按钮 | 类型 | 位置 | 行为 |
|------|------|------|------|
| 新增字典 | primary | toolbar-right | navigate → /settings/dictionaries/new |
| 查看 | text | row | navigate → /settings/dictionaries/:id |
| 编辑 | text | row | navigate → /settings/dictionaries/:id/edit |

##### 业务规则

- 字典编码全局唯一
- 系统内置字典不可删除
- 字典项支持排序和颜色配置

---

## 四、全局规则

### 4.1 角色权限

| 角色 | 描述 | 模块权限 |
|------|------|----------|
| 系统管理员 | 负责系统配置、用户管理、角色权限管理 | 全部模块权限 |
| 设备管理员 | 负责设备台账管理、维护计划制定、报表分析 | 设备台账(全部)、工单管理(查看)、预防性维护(全部)、备品备件(查看)、报表分析(全部)、系统设置(无) |
| 维修工程师 | 负责故障维修、预防性维护执行 | 设备台账(查看)、工单管理(全部)、预防性维护(查看)、备品备件(查看)、报表分析(查看)、系统设置(无) |
| 设备操作工 | 负责日常设备点检、故障报修 | 设备台账(查看)、工单管理(创建/查看我的工单)、预防性维护(无)、备品备件(无)、报表分析(无)、系统设置(无) |
| 采购仓储 | 负责备品备件采购和库存管理 | 设备台账(无)、工单管理(查看)、预防性维护(无)、备品备件(全部)、报表分析(查看)、系统设置(无) |

### 4.2 数据字典

#### dict-equipment-category (设备分类)

| 值 | 显示 | 颜色 |
|----|------|------|
| metal-working | 金属切削设备 | blue |
| forming | 成形设备 | blue |
| heat-treatment | 热处理设备 | blue |
| assembly | 装配设备 | blue |
| testing | 检测设备 | blue |
| other | 其他设备 | gray |

#### dict-equipment-status (设备状态)

| 值 | 显示 | 颜色 |
|----|------|------|
| running | 运行 | success |
| stopped | 停机 | warning |
| maintenance | 维修 | error |
| scrapped | 报废 | default |

#### dict-location (位置)

| 值 | 显示 | 颜色 |
|----|------|------|
| workshop-a | A车间 | blue |
| workshop-b | B车间 | blue |
| workshop-c | C车间 | blue |
| warehouse | 仓库 | green |

#### dict-work-order-status (工单状态)

| 值 | 显示 | 颜色 |
|----|------|------|
| pending | 待处理 | warning |
| processing | 处理中 | processing |
| completed | 已完成 | success |
| closed | 已关闭 | default |

#### dict-priority (优先级)

| 值 | 显示 | 颜色 |
|----|------|------|
| high | 高 | error |
| medium | 中 | warning |
| low | 低 | default |

#### dict-work-order-type (维修类型)

| 值 | 显示 | 颜色 |
|----|------|------|
| corrective | 纠正性维修 | blue |
| preventive | 预防性维护 | green |

#### dict-trigger-type (触发方式)

| 值 | 显示 | 颜色 |
|----|------|------|
| time | 时间 | blue |
| meter | 计量值 | green |

#### dict-maintenance-type (维护类型)

| 值 | 显示 | 颜色 |
|----|------|------|
| daily | 日保 | blue |
| weekly | 周保 | blue |
| monthly | 月保 | blue |
| yearly | 年保 | blue |

#### dict-plan-status (计划状态)

| 值 | 显示 | 颜色 |
|----|------|------|
| enabled | 启用 | success |
| disabled | 停用 | default |

#### dict-spare-part-category (备件分类)

| 值 | 显示 | 颜色 |
|----|------|------|
| electrical | 电气件 | blue |
| mechanical | 机械件 | blue |
| hydraulic | 液压件 | blue |
| pneumatic | 气动件 | blue |
| consumable | 易耗品 | orange |
| other | 其他 | gray |

#### dict-stock-status (库存状态)

| 值 | 显示 | 颜色 |
|----|------|------|
| normal | 正常 | success |
| low | 低库存 | warning |
| out | 缺货 | error |

#### dict-unit (单位)

| 值 | 显示 | 颜色 |
|----|------|------|
| piece | 件 | default |
| set | 套 | default |
| meter | 米 | default |
| kg | 千克 | default |
| liter | 升 | default |

#### dict-user-status (用户状态)

| 值 | 显示 | 颜色 |
|----|------|------|
| enabled | 启用 | success |
| disabled | 禁用 | default |

### 4.3 状态流转

#### 工单状态流转

| 当前状态 | 操作 | 目标状态 | 条件 |
|----------|------|----------|------|
| 待处理 | 派工 | 处理中 | 指定维修人 |
| 处理中 | 完成维修 | 已完成 | 填写维修记录 |
| 已完成 | 关闭 | 已关闭 | 无 |
| 已完成 | 重新打开 | 处理中 | 管理员权限 |
| 待处理 | 取消 | 已关闭 | 管理员权限 |

#### 维护计划状态流转

| 当前状态 | 操作 | 目标状态 | 条件 |
|----------|------|----------|------|
| 启用 | 停用 | 停用 | 无 |
| 停用 | 启用 | 启用 | 无 |

---

## 附录

### A. 变更记录

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| 1.0.0 | 2026-03-26 | 初始版本,基于宜昌长机科技EAM系统解决方案生成 |
