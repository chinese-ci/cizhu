# ADR 0001：词铸 CiZhu 文档、词模 CiMo 契约、工具独立

- 状态：已接受
- 日期：2026-11-21

## 背景

早期实现把采集、清洗、构建、规格与验收记录放在同一个私有仓库。随着清洗与构建拆为独立工具，需要一个中立的工程文档与契约归属，否则跨仓库兼容性无法追踪，公开侧也会混入来源细节。

## 决策

1. 词铸 CiZhu 是公开工程门面：README、架构、规格、ADR、术语、社区、版本矩阵、仓库地图。
2. 词模 CiMo 只放规范性契约与机器可读模型：source-corpus、material、taxonomy、target adapter、[rime-wubi](https://github.com/rime-wubi/rime-wubi) target、versioning。
3. 词采 CiCai、词修 CiXiu、词合 CiHe 是纯工具仓库。工具之间通过契约与 CLI 边界连接，不互相 import 运行时私有结构。
4. 词采 CiCai 私有，集中保存平台适配、原始文件、来源类目、来源映射与策展覆盖。
5. 默认目标产物仓库独立：`rime-wubi/rime-wubi`，与源码组织 `chinese-ci` 分离。

## 后果

- 公开读者从词铸 CiZhu 理解全貌，从[词模 CiMo](https://github.com/chinese-ci/cimo) 读契约，不需要接触来源细节。
- 契约变更必须同时更新[词模 CiMo](https://github.com/chinese-ci/cimo)、vendored 副本与版本矩阵。
- 多仓库需要链接与版本纪律；用完整 GitHub URL 与 vendored 哈希降低漂移。

## 备选

- 继续单仓库：工具无法独立发布，公开/私有边界难守。
- 只拆契约仓库、不拆文档：工程叙事仍会与工具代码混杂。
