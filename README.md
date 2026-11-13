# Jsgrow JSON:API Contract

这是 Jsgrow 前后端共同维护的 JSON:API 契约仓库。它定义资源、关系、操作、错误、分页、身份和版本兼容规则；具体实现属于 `jsgrow` 后端，界面和交互属于 `jsgrow-wap` 前端。

当前版本：**core v0.1 draft**。这是讨论稿，不代表已经锁定。前后端在 Discussion 中逐项确认后，才将版本标记为 `accepted` 并允许实现依赖它。

## 仓库边界

- 核心契约包含论坛、用户身份、普通私信、核心通知、权限、站点配置和通用能力发现。
- 客服、工单、扩展通知、支付宝等属于插件；每个插件单独维护契约文件和版本记录。
- 商品、需求、支付、履约和结算使用通用资源与关系表达业务装配，经营比例、文案、排序和开关属于站点配置。
- 契约不把某个站点的频道名称、分类文字、导航排序或业务流程写成代码枚举。

## 评审流程

1. 前后端在本仓库的 Issue 或 Discussion 中提出字段、关系和状态变更。
2. 维护者提交契约变更 PR，并在变更说明中列出兼容性影响和 mock 数据。
3. `core` 变更必须由前端和后端各自确认；破坏性变更必须增加主版本并说明迁移方式。
4. 契约接受后，后端实现和 WAP 实现分别在各自仓库引用已接受的版本；未实现接口返回契约规定的 `planned` 元数据或 mock 响应，不让前端猜测字段。

## 文件

- [`contracts/core.v0.1.json`](contracts/core.v0.1.json)：机器可读的核心资源、关系、操作和状态草案。
- [`docs/core-v0.1.md`](docs/core-v0.1.md)：字段语义、权限矩阵、分页、错误和 mock 约定。
- [`CHANGELOG.md`](CHANGELOG.md)：版本和破坏性变更记录。

实现仓库：[`xiaolanidle-cyber/jsgrow`](https://github.com/xiaolanidle-cyber/jsgrow)

前端仓库：[`xiaolanidle-cyber/jsgrow-wap`](https://github.com/xiaolanidle-cyber/jsgrow-wap)
