


### promotion-status-card
原状态：
豁免列表中![[Pasted image 20260528160352.png]]

![[Pasted image 20260528170556.png]]

这次测试里哪些是纯增量
- footer 插槽渲染
- 无 footer 时不渲染底部区域
哪些是顺带补的回归覆盖
- 标题后置插槽显隐
- 描述默认样式
- suggestionPrefix / descSuffix
- hint-click / visible-change / suggest-click
13m 53s
#### 本轮待补行为
- 默认态未传 `footer` slot 时，不渲染底部容器
- 传入 `footer` slot 时，渲染底部内容与底部容器类
![[Pasted image 20260526114008.png]]
![[Pasted image 20260526113737.png]]
test-detect误报
test-detect无法正确处理单引号字符串


## create-select-ies
#### 本轮待补行为
- 展开下拉框时触发 `visible-change`
- `resetOnOpen=true` 时每次展开都会重新请求列表
- `reset(params, { clearList: false })` 会等待在飞请求结束，并保留当前列表直到新请求完成
- `resetPage()` 只重置页码，不清空当前选项
![[Pasted image 20260526133810.png]]
![[Pasted image 20260526133832.png]]
要求AI对照设计方案补充后
![[Pasted image 20260526145502.png]]


## oc-table
#### 待补代码
- 存量代码全覆盖
#### 覆盖率情况
- 存量代码情况
![[Pasted image 20260527114716.png]]
- 补测前情况
![[Pasted image 20260527112950.png]]
- 补测后情况
![[Pasted image 20260527113912.png]]
#### 补充细节
已测试功能
- showExtraRow 数组模式：只为命中的行展示额外操作行，未命中的行不展示，见 oc-table.test.ts 。
- renderExtraRowCell ：额外操作行能按列配置渲染自定义内容，见 oc-table.test.ts 。
- extraColspan ：额外操作行单元格支持合并列，已断言 colspan="2" ，见 oc-table.test.ts 。
- extraClass ：额外 tr 和额外 td 都能透传 class，见 oc-table.test.ts 。
- 结构兼容性：额外操作行与展开列、选择列、固定列共存时仍能正确渲染占位和内容，见 oc-table.test.ts 。
- showExtraRow: true ：所有数据行都展示额外操作行，见 oc-table.test.ts 。
- extraRowClick 、 extraRowMouseenter 、 extraRowMouseleave ：三个新增事件都已验证，且断言了 rowData.id 与 rowIndex ，见 oc-table.test.ts 。
未覆盖问题：
- 空态区域存在时同时 loading === true
#### 结论
增量代码完全覆盖，存在问题属于存量代码
执行时间：2m39s

## create-material-upload-video
没有遵循技能要求将组件从豁免列表中移除
- 修复：要求AI在行动前必须检查，本次流程中已解决
#### 覆盖率情况
- 存量代码情况
在豁免列表
- 补测前情况
存在未通过单测
- 补测后情况
![[Pasted image 20260528112839.png]]
#### 补充细节
增量补充，但是为了通过test-detect编写了大量命名型断言
test-detect生成期望断言名有问题，无法处理表达式 `testHideUploadImageModeListLength >1`
#### 结论
业务组件存在大量未覆盖缺口和遗留问题
执行时间：22m 18s（模型意外中断）

## oc-row

#### 覆盖率情况
- 存量代码情况
![[Pasted image 20260528141044.png]]
- 补测前情况
![[Pasted image 20260528140955.png]]
- 补测后情况
![[Pasted image 20260528140917.png]]
- 进行一次补充
![[Pasted image 20260528144242.png|697]]
- 全量补测情况
![[Pasted image 20260528145143.png]]
#### 补充细节

#### 结论
AI有能力补全组件单测覆盖，但是在增量模式下对于代码中需要补全的覆盖点可能存在模糊
执行时间：4m 30s（增量），5m 31s（全量）


test: PromotionStatusCard 增加底部插槽:  小
1a4baa6ee80fe06dd7620017affdfbec47e23df3

test: OcSelect 搜索后全选只作用于当前过滤结果 小
38badfeb9cac9bce3bceea95936d9dc8ab755261

test: CreateSelectIes 优化展开重拉、分页重置和 reset 竞态 中
d40bba5b1e3e4b4cb72881d4f4db0eae38b2855e

test: CreateMaterialUploadVideo 支持上传状态筛选、并发限制和关闭/提交拦截 大
8e2bf62cf88e20e2b17124585454497eab28b31f

test: OcTable 支持数据行下方的额外操作行 大
865447c6b32195b2182798073c62441020353cff

test: OcRow 支持横向布局与标题尺寸配置 大
980c57d56682532d7f573744aa4e99e7e03e6dfb