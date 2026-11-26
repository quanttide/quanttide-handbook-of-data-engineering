# 量潮数据工程手册

量潮数据工程标准的格式规范仓（quanttide-handbook-of-data-engineering）。

本仓回答"长什么样、放在哪、怎么写、怎么校验"：配置目录、命名规范、路径解析、环境变量，以及各组件的字段结构、验证规则、模板与示例。

概念、关系、状态与类型等本体定义见 specification 仓（quanttide-specification-of-data-engineering）。

## 结构

```
index.md                     配置目录、路径解析、环境变量、文档地图
requirement/index.md         DRD 文档格式（章节、模板、示例）
specification/contract.md    数据契约 YAML 格式
specification/blueprint.md   数据蓝图 YAML 格式
implementation/catalog.md    数据目录 JSON 格式
implementation/pipeline.md   数据管道 YAML 格式
```

本仓格式内容自 specification 仓分解而来，分解记录见该仓 CHANGELOG。
