# Plugin contract template

每个插件使用独立文件，例如 `customer-service.v1.0.0.json`。插件不得修改 core 资源的必填字段或状态语义；如果必须增加关系或操作，应在插件契约中声明兼容范围和回退行为。

最小字段：

```json
{
  "contract": "jsgrow.plugin.example",
  "version": "1.0.0",
  "status": "draft",
  "requiresCore": ">=0.1.0",
  "resources": {},
  "operations": {},
  "breakingChanges": []
}
```

插件启停不会删除数据。停用后，前端通过能力表隐藏入口；历史资源应能以只读或明确的 unavailable 状态展示。
