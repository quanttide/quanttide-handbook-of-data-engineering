# Catalog — 数据目录（格式）

> 格式：JSON

Catalog 的定义（运行时文件注册表，回答"有哪些数据"）与 Volume 状态机（`status` 值域与合法转移）见 specification 仓（quanttide-specification-of-data-engineering）`implementation/catalog.md`。

---

## 1. 格式与位置

### 1.1 格式

Catalog MUST 使用 JSON 格式。Catalog 的受众是运行时和审计方，MUST 可被机器解析和验证。

### 1.2 文件位置

Catalog 文件 MUST 存放在 `.quanttide/data/catalog/registry.json`。可通过 `CATALOG_DIR` 环境变量覆盖。

---

## 2. 结构定义

### 2.1 顶层键

| 字段 | 类型 | 约束 | 说明 |
|------|------|------|------|
| 键 | string | MUST 存在，MUST 唯一 | Volume 名称，即注册表条目键 |
| 值 | object | MUST 存在 | Volume 记录。字段定义见 2.2 节 |

### 2.2 Volume 记录字段

| 字段 | 类型 | 约束 | 说明 |
|------|------|------|------|
| `name` | string | MUST 存在，MUST 与顶层键一致 | Volume 名称 |
| `path` | string | MUST 存在，MUST 为绝对路径 | 文件绝对路径 |
| `size` | number | MUST 存在，MUST 非负 | 文件大小（字节） |
| `received_at` | datetime | MUST 存在 | 注册时间。格式 MUST 遵循 Contract 的 `datetime` 类型（`YYYY-MM-DDTHH:MM:SSZ`） |
| `status` | enum | MUST 存在 | 状态。值域与合法转移见 specification 仓 catalog.md 状态机 |
| `provider` | string | MAY 存在 | 来源 provider |
| `source` | string | MAY 存在 | 来源 URL |

---

## 3. 验证规则

符合格式规范的 Catalog MUST 满足以下规则：

1. 顶层键 MUST 非空且唯一
2. 每个 Volume 记录的 `name` MUST 与顶层键一致
3. `status` MUST 在 specification 仓 catalog.md 状态机定义的值域内
4. `path` MUST 为绝对路径
5. `received_at` MUST 遵循 2.2 节定义的时间格式

状态变更记录不在 Catalog 中持久化（Catalog 只保存当前状态）；转移合法性由写入方（Pipeline 实现）负责校验，规则见 specification 仓 catalog.md 状态机。

---

## 4. 结构

```json
{
  "<volume-name>": {
    "name": "<volume-name>",
    "path": "<绝对路径>",
    "size": <字节数>,
    "received_at": "<YYYY-MM-DDTHH:MM:SSZ>",
    "status": "received|processing|processed|delivered",
    "provider": "<来源 provider>",
    "source": "<来源 URL>"
  }
}
```

---

## 5. 示例

```json
{
  "cust-001-raw": {
    "name": "cust-001-raw",
    "path": "/tmp/qtcloud-data/cust-001/raw.csv",
    "size": 2340000,
    "received_at": "2026-07-10T12:00:00Z",
    "status": "received",
    "provider": "dropbox",
    "source": "https://www.dropbox.com/s/xxx/file.csv"
  }
}
```
