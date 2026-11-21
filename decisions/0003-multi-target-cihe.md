# ADR 0003：词合 CiHe 为多 target 构建器，v1 只交付 rime-wubi

- 状态：已接受
- 日期：2026-11-21

## 背景

[词合 CiHe](https://github.com/chinese-ci/cihe) 目前只构建 Rime 方案，但底层编码器支持多种编码与输出格式。社区可能希望同一份物料产出其它输入法所需文件。

## 决策

1. [词合 CiHe](https://github.com/chinese-ci/cihe) 的定位是“物料 → target 适配器”；默认 target 为 `rime-wubi`。
2. v1 只实现并验收 `rime-wubi`；其它 target 必须显式指定，不进入首版验收。
3. target 适配器负责 encode、emit、verify；可调用外部编码器。适配器只生成由物料派生的 Ci 内容片段，不生成目标项目的基础结构。
4. 契约分为通用 `targets/adapter` 与具体 `targets/rime-wubi`；[rime-wubi](https://github.com/rime-wubi/rime-wubi) 的 schema、字表、主表与发布文件由产物仓库维护。
5. 默认目标产物发布在独立组织 `rime-wubi`；[词合 CiHe](https://github.com/chinese-ci/cihe) 源码留在 `chinese-ci`，v1 不配置跨组织自动推送。字表工具单独放在 `rime-wubi/zi`。

## 后果

- 新增输入法目标不需要改物料或[词修 CiXiu](https://github.com/chinese-ci/cixiu)，只需要新适配器与目标契约。
- 首版验收范围保持可控；多 target 的兼容矩阵按需扩展。
- 目标产物的许可与发布节奏可以与源码分离。
