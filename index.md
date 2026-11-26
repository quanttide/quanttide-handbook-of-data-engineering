# 量潮数据工程手册

> 本仓定义数据工程组件的**格式规范**——文件长什么样、放在哪、怎么写、怎么校验。
> 概念、关系、状态与类型等本体定义见 specification 仓（quanttide-specification-of-data-engineering）。
> 本仓关键词 `MUST`、`SHOULD`、`MAY` 遵循 [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt)，与 specification 仓一致。

---

## 1. 配置目录

本节遵循"约定优先于配置"原则。用户无需手动设置即可使用，但所有路径 MUST 可通过环境变量覆盖。

### 1.1 目录结构

符合规范的实现 MUST 使用以下目录布局：

```
.quanttide/data/
├── requirement/         # 数据需求文档（Markdown）
├── specification/       # 数据规格书
│   ├── contract/        #   数据契约（YAML）
│   └── blueprint/       #   数据蓝图（YAML）
├── catalog/             # 数据目录（registry.json）
└── pipeline/            # 数据管道（YAML + 可执行代码）
```

### 1.2 命名规范

所有目录名和配置字段名 MUST 遵循 Unix 风格：

- **单数** — 目录名用单数。示例: `contract/`（正确），`contracts/`（错误）
- **小写** — 全小写字母。示例: `blueprint/`（正确），`BluePrint/`（错误）
- **连字符** — 单词间用 `-` 分隔。示例: `requirement/`（正确），`requirements/`（错误）
- **无缩写** — 除非是通用缩写。示例: `catalog/`（正确），`catlg/`（错误）

YAML 文件中的字段名 MUST 遵循相同规范：

```yaml
# 正确
name: csv-standard
steps:
  - command: python3

# 错误
name: csvStandard
Steps:
  - Command: python3
```

### 1.3 项目根与路径解析

**项目根目录**解析顺序 MUST 按以下优先级进行：

1. 环境变量 `PROJECT_DIR`（若设置）
2. 从当前工作目录向上查找最近的包含 `.quanttide/` 的祖先目录
3. 未找到时，当前工作目录视为项目根目录

**组件路径**解析 MUST 按以下优先级进行：

1. 同名环境变量（如 `PIPELINE_DIR`）
2. 环境变量未设置时使用 [1.1 节](#11-目录结构) 定义的默认路径
3. 默认路径相对于当前工作目录解析

### 1.4 环境变量

| 环境变量 | 默认值 | 说明 |
|----------|--------|------|
| `REQUIREMENT_DIR` | `.quanttide/data/requirement` | DRD 目录 |
| `SPECIFICATION_DIR` | `.quanttide/data/specification` | 规格书根目录 |
| `CONTRACT_DIR` | `.quanttide/data/specification/contract` | Contract 目录 |
| `BLUEPRINT_DIR` | `.quanttide/data/specification/blueprint` | Blueprint 目录 |
| `CATALOG_DIR` | `.quanttide/data/catalog` | Catalog 目录 |
| `PIPELINE_DIR` | `.quanttide/data/pipeline` | Pipeline 目录 |

---

## 2. 文档地图：规范仓库 vs 运行时目录

本仓按四层框架组织**格式文档**；格式本身定义的是各实现项目中的**运行时目录**（`.quanttide/data/`）。两者同名但不同义，对应关系如下：

| 格式文档（本仓） | 运行时目录（`.quanttide/data/`） | 内容 |
|-------------------|--------------------------------|------|
| `requirement/index.md` | `requirement/` | DRD 文档（Markdown） |
| `specification/contract.md` | `specification/contract/` | 数据契约（YAML） |
| `specification/blueprint.md` | `specification/blueprint/` | 数据蓝图（YAML） |
| `implementation/catalog.md` | `catalog/` | 数据目录（registry.json） |
| `implementation/pipeline.md` | `pipeline/` | 数据管道（YAML + 代码） |

注意区分：
- 本仓的 `specification/` 目录存放**格式文档**（.md），描述 Contract 和 Blueprint 长什么样
- 运行时目录 `specification/` 存放**规范实例**（.yaml），是具体项目中的契约和蓝图文件
- 同理，本仓的 `implementation/` 目录存放 Catalog/Pipeline 的格式文档，运行时没有 `implementation/` 这一层（catalog/ 与 pipeline/ 直接位于 `.quanttide/data/` 下）

---

## 3. 组件格式文档

| 组件 | 格式文档 | 格式 |
|------|---------|------|
| DRD | [requirement/index.md](requirement/index.md) | Markdown |
| Contract | [specification/contract.md](specification/contract.md) | YAML |
| Blueprint | [specification/blueprint.md](specification/blueprint.md) | YAML |
| Catalog | [implementation/catalog.md](implementation/catalog.md) | JSON |
| Pipeline | [implementation/pipeline.md](implementation/pipeline.md) | YAML + 可执行代码 |
