


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

## oc-select
存量情况
![[Pasted image 20260528214240.png]]
补测前
![[Pasted image 20260528213950.png]]
增量补测
![[Pasted image 20260528213248.png]]
增量补测且附有需求方案
6m 40s
全量补测
![[Pasted image 20260529145910.png]]
12m 3s（存在提问耗时）
#### 补充细节
- 未覆盖点 1：“过滤结果为空时，全选状态直接返回 none ”这条分支没测到。 550
	- 增量、带方案增量均未覆盖，全量覆盖
	- 需求方案描述：`禁用项和 disabledValues 仍然不能被选中`
- 未覆盖点 2：渲染里有一个三元分支只命中了一侧，实际缺的是 isValidText ? 'default' : 'secondary' 的另一侧，也就是 secondary 那条分支没覆盖到。 68-75
	- `v-else-if`导致的不可达分支



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
补过的场景
- 额外操作行渲染与按行过滤
  - 验证 showExtraRow 传入数组时，只在命中的行下方渲染额外行
  - 验证 extraClass.extraRow 和 extraClass.extraTd 能正确挂到额外行/单元格上
  - 验证 extraColspan 的合并列效果是否生效
- 额外操作行事件透传
  - 验证 extra-row-click
  - 验证 extra-row-mouseenter
  - 验证 extra-row-mouseleave
  - 顺带验证点击额外操作行时，不会误触发普通 row-click
补测目的
- 覆盖这次提交新增的“数据行下方额外操作行”渲染能力
- 覆盖新增的 3 个额外行事件
- 补齐额外行和普通数据行之间的行为边界
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

test: cSelect 搜索后全选只作用于当前过滤结果 小
38badfeb9cac9bce3bceea95936d9dc8ab755261

test: CreateSelectIes 优化展开重拉、分页重置和 reset 竞态 中
d40bba5b1e3e4b4cb72881d4f4db0eae38b2855e

test: CreateMaterialUploadVideo 支持上传状态筛选、并发限制和关闭/提交拦截 大
8e2bf62cf88e20e2b17124585454497eab28b31f

test: OcTable 支持数据行下方的额外操作行 大
865447c6b32195b2182798073c62441020353cff

test: OcRow 支持横向布局与标题尺寸配置 大
980c57d56682532d7f573744aa4e99e7e03e6dfb