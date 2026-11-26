# CHANGELOG

所有显著变更都将记录在此文件中。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)。

版本遵循语义化版本规范：0.0.x（探索期）→ 0.x.y（验证期）→ x.y.z（正式期）

---

## [0.1.0] - 2026-11-26

### Added

- 格式规范自 specification 仓（quanttide-specification-of-data-engineering）分解入本仓，本仓回答"长什么样、放在哪、怎么写、怎么校验"：
  - `index.md`：配置目录（目录结构、命名规范、项目根与路径解析、环境变量）、文档地图（规范仓库 vs 运行时目录）、组件格式文档导航
  - `requirement/index.md`：DRD 文件格式与位置、章节结构、模板、示例
  - `specification/contract.md`：Contract YAML 字段结构、验证规则、结构模板、示例
  - `specification/blueprint.md`：Blueprint YAML 字段结构、验证规则、结构模板、示例
  - `implementation/catalog.md`：Catalog JSON 字段结构、验证规则、结构模板、示例
  - `implementation/pipeline.md`：Pipeline YAML 字段结构、验证规则、结构模板、示例
