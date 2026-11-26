# Blueprint — 数据蓝图（格式）

> 格式：YAML

Blueprint 的定义、与 Contract 的关联关系，以及步骤执行语义（串行顺序、`depends` 优先、`from`/`to` 数据流）见 specification 仓（quanttide-specification-of-data-engineering）`specification/blueprint.md`。

---

## 1. 格式与位置

### 1.1 格式

Blueprint MUST 使用 YAML 格式。Blueprint 的受众是工程师和 Pipeline 实现者，MUST 可被机器解析。

### 1.2 文件位置

Blueprint 文件 MUST 存放在 `.quanttide/data/specification/blueprint/` 目录下。

文件命名 SHOULD 使用蓝图名称，如 `<blueprint-name>.yaml`。

---

## 2. 结构定义

### 2.1 顶层字段

| 字段 | 类型 | 约束 | 说明 |
|------|------|------|------|
| `name` | string | MUST 存在，MUST 唯一 | 蓝图名称 |
| `description` | string | SHOULD 存在 | 整体处理目标的业务化描述 |
| `contract` | string | MUST 存在 | 关联的 Contract 名称。蓝图处理的数据结构由该 Contract 定义 |
| `steps` | list | MUST 存在，MUST NOT 为空 | 处理步骤的有序列表。执行顺序从上到下 |

### 2.2 Step 定义（steps 中的每个条目）

| 字段 | 类型 | 约束 | 说明 |
|------|------|------|------|
| `name` | string | MUST 存在 | 步骤名称。面向业务的简短描述（如"数据格式校验"） |
| `description` | string | SHOULD 存在 | 该步骤的业务逻辑说明。描述做了什么、规则是什么、不合规数据如何处理 |
| `expectation` | string | SHOULD 存在 | 该步骤完成后的预期产出。是描述性的验收预期（供人工或工具复核），不构成自动断言 |
| `from` | string | MAY 存在 | 该步骤的输入数据（数据流语义） |
| `to` | string | MAY 存在 | 该步骤的输出数据（数据流语义） |
| `depends` | list<string> | MAY 存在 | 前置步骤名称列表。执行时 MUST 先于依赖步骤完成 |

步骤执行语义（串行顺序、`depends` 与列表顺序冲突时以 `depends` 为准、`from`/`to` 数据流衔接）见 specification 仓 blueprint.md。

---

## 3. 验证规则

符合格式规范的 Blueprint MUST 满足以下规则：

1. `name` 字段 MUST 非空且唯一
2. `contract` 字段 MUST 引用一个已存在的 Contract 名称（解析规则见 specification 仓 index.md §1.5 名称与引用解析）
3. `steps` MUST 为非空列表
4. 每个 step 的 `name` MUST 非空
5. `depends` 中引用的步骤名 MUST 存在于本 Blueprint 的 `steps` 中，且不得自引用

---

## 4. 结构

```yaml
name: <blueprint-name>
description: <整体处理目标>
contract: <关联的 Contract 名称>

steps:
  - name: <步骤1名称>
    description: <业务逻辑说明>
    expectation: <预期产出>
    from: <输入数据>
    to: <输出数据>

  - name: <步骤2名称>
    description: <业务逻辑说明>
    expectation: <预期产出>
    from: <输入数据>
    to: <输出数据>
    depends:
      - <步骤1名称>
```

完整示例见[第 5 节](#5-示例)。

---

## 5. 示例

```yaml
name: ghtorrent-user-activity
description: 从 GHTorrent MySQL dump 中提取、清洗、聚合用户每日活动数据
contract: ghtorrent-user-activity

steps:
  - name: 环境搭建与数据下载
    description: 配置计算环境，从指定源下载 MySQL dump 文件
    expectation: 可访问的原始数据文件

  - name: 数据解压与提取
    description: 解压 dump 文件，提取用户表、事件表等关键 CSV
    expectation: 结构化的原始 CSV 文件集

  - name: 用户匹配与数据筛选
    description: 用目标用户 ID 列表匹配用户表获取内部 ID，以内部 ID 为核心过滤各事件表
    expectation: 仅包含目标用户相关记录的子集

  - name: 多维度日聚合计数
    description: 按用户-日期分组，聚合 push、PR、Issue 评论等维度的日计数。Bot 分类器并行标注
    expectation: 每日每用户一行，含所有活动维度计数和 bot 标记

  - name: 面板合并
    description: 合并所有用户的面板数据为单一面板数据集
    expectation: 统一格式的面板 CSV

  - name: 数据验证与交付
    description: 验证列数、去重、bot 标记一致性。生成《数据处理说明》
    expectation: 通过验收的交付物（CSV + 处理说明文档）
```
