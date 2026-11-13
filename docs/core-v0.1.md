# Core v0.1 语义草案

## 1. 通用 JSON:API 约定

- 请求和响应使用 `application/vnd.api+json`，路径前缀为 `/api`。
- 资源使用稳定的 `type` 和 `id`。字段使用 camelCase；数据库列名不暴露给客户端。
- 关系使用 JSON:API `relationships`，不把用户、帖子或订单 ID 拼进业务文案。
- 时间使用 RFC 3339 UTC 字符串；金额使用 JSON 安全范围内的整数，交易金额单位为人民币分，币种固定 `CNY`。
- 写操作需要认证、权限和资源关系同时通过。客户端提供的 actor、owner、payer 或 recipient ID 不作为授权依据。
- 幂等写操作通过资源自身版本或请求键保护；重试条件不同返回 `409`。
- 列表默认返回 `meta.nextAfter`。游标只由服务端生成，客户端不得修改或解码后自造。

## 2. 错误与未实现接口

错误使用 JSON:API `errors` 数组，至少包含 `status`、`code` 和可展示的 `detail`。客户端按 `code` 处理，不按中文 detail 判断逻辑。

契约中状态为 `planned` 的接口在后端尚未实现时，开发环境可以返回：

```json
{
  "meta": {
    "contract": "jsgrow.core@0.1.0-draft",
    "contractStatus": "planned",
    "mock": true,
    "message": "This operation is defined by the contract but is not enabled on this site."
  },
  "data": []
}
```

生产环境可以返回标准 `501 not_implemented`，但必须保留 `meta.contract` 和 `meta.contractStatus`，不允许直接返回未说明的 404 或 HTML 错误页。

## 3. 核心身份与权限

| 身份 | 能力范围 |
| --- | --- |
| 游客 | 公开用户摘要、公开分类、公开论坛和帖子、能力发现 |
| 注册用户 | 自己的资料、发帖/回复、普通私信、自己的通知、允许的商品/服务操作 |
| 参与者 | 自己参与的订单、服务过程和结果；由资源关系进一步限制 |
| 管理员 | 站点设置、插件启停、审核、报告和所有配置管理 |
| 服务边界 | 只能使用明确注册的内部操作，不代表自动拥有 HTTP 写权限 |

## 4. 普通私信（核心）

普通私信从用户个人空间或帖子作者入口建立。最小字段为 `sender`、`recipient`、`body`、`createdAt`、`readAt`、`retractedAt` 和可选 `attachments` 关系。打开单条消息、客服消息或服务通知内容都等价于已读；前端不需要额外点击“已读”按钮才能改变状态。

私信支持软撤回和并发安全的已读更新。客服、工单和模板化扩展通知不进入核心资源，通过插件契约扩展 `notifications` 或独立资源。

## 5. 分类与内容

`content-products.kind` 是站点配置的稳定类型码，不在核心代码写死文章、资源、问答、服务或需求。管理员可以增加、停用和排序类型；前端从配置或能力表读取展示文字。内容是否免费、会员或付费由内容属性/插件能力表达，不能用前端入口名称推断。

## 6. 版本锁定

接受版本后，以下变化属于破坏性变更：删除或重命名字段、改变字段类型、改变关系基数、改变状态语义、改变权限前置条件。新增可选字段、资源或操作属于兼容性变更，但仍需更新契约版本和 mock 数据。

插件契约使用独立版本，例如 `jsgrow.plugin.customer-service@1.0.0`，不改变 core 版本。每次插件契约变更必须说明影响的 WAP 路由、组件和回退行为。
