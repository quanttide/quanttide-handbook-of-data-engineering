# Contract — 数据契约（格式）

> 格式：YAML

Contract 的定义、input/output 方向语义与类型系统（`type` 合法值、空值语义）见 specification 仓（quanttide-specification-of-data-engineering）`specification/contract.md`。

---

## 1. 格式与位置

### 1.1 格式

Contract MUST 使用 YAML 格式。Contract 的受众是工程师，MUST 可被机器解析和验证。

### 1.2 文件位置

Contract 文件 MUST 存放在 `.quanttide/data/specification/contract/` 目录下。

文件命名 SHOULD 使用契约名称，如 `<contract-name>.yaml`。

---

## 2. 结构定义

### 2.1 顶层字段

| 字段 | 类型 | 约束 | 说明 |
|------|------|------|------|
| `name` | string | MUST 存在，MUST 唯一 | 契约名称，用于 Blueprint 引用 |
| `description` | string | SHOULD 存在 | 契约用途的自然语言描述 |
| `input` | map | MUST 存在 | 输入字段定义。键为字段名 |
| `output` | map | MUST 存在 | 输出字段定义。键为字段名 |

### 2.2 字段定义（input / output 中的每个条目）

| 字段 | 类型 | 约束 | 说明 |
|------|------|------|------|
| `type` | string | MUST 存在 | 数据类型。合法值见 specification 仓 contract.md 类型系统 |
| `doc` | string | MUST 存在 | 字段的业务含义描述 |
| `constraint` | string | MAY 存在（input 字段 SHOULD 提供） | 输入约束条件。描述格式要求、值域范围、必填/可选等 |
| `guarantee` | string | MAY 存在（output 字段 SHOULD 提供） | 输出质量承诺。描述去重率、空值率、精度要求等 |

---

## 3. 验证规则

符合格式规范的 Contract MUST 满足以下规则：

1. `name` 字段 MUST 非空且在该项目的所有 Contract 中唯一
2. `input` 中每个字段的 `type` MUST 在 specification 仓 contract.md 类型系统的合法值列表中
3. `output` 中每个字段的 `type` MUST 在 specification 仓 contract.md 类型系统的合法值列表中
4. `type` 为 `enum` 的字段 MUST 附带 `values` 子字段，`values` MUST 为非空列表
5. 字段名 MUST 遵循 [index.md](../index.md#12-命名规范) 的 Unix 命名规范

---

## 4. 结构

```yaml
name: <contract-name>
description: <用途描述>

input:
  <field-name>:
    type: <string|number|integer|boolean|date|datetime|enum>
    doc: <业务含义>
    constraint: <约束条件>

output:
  <field-name>:
    type: <string|number|integer|boolean|date|datetime|enum>
    doc: <业务含义>
    guarantee: <质量承诺>
```

完整示例见[第 5 节](#5-示例)。

---

## 5. 示例

```yaml
name: ghtorrent-user-activity
description: GHTorrent 用户活动面板数据的输入输出契约

input:
  mysql_dump:
    type: string
    doc: GHTorrent MySQL dump 文件路径
    constraint: required, must be a valid file path
  user_list:
    type: string
    doc: 目标用户 ID 列表文件路径
    constraint: required, CSV format with one ID per line

output:
  anonymized_id:
    type: string
    doc: 去标识化的用户标识符
    guarantee: unique per user, non-empty, deterministic hash
  activity_date:
    type: date
    doc: 活动观察日期
    guarantee: non-null, YYYY-MM-DD format
  push_count:
    type: integer
    doc: 当日 push 事件数
    guarantee: non-negative
  pr_count:
    type: integer
    doc: 当日 PR 事件数
    guarantee: non-negative
  is_bot:
    type: boolean
    doc: 是否为 bot 账户
    guarantee: determined by validated classifier, not heuristic rules
```
