# ADR 0004：物料类目是契约，不是工具内部数据

- 状态：已接受
- 日期：2026-11-21

## 背景

类目树、文件、slug 与顺序同时被[词修 CiXiu](https://github.com/chinese-ci/cixiu)、[词合 CiHe](https://github.com/chinese-ci/cihe)、验收与物料仓库消费。若留在某个工具内部，其他仓库只能复制或猜测，类目一改就多处漂移。

## 决策

1. 物料类目正本放[词模 CiMo](https://github.com/chinese-ci/cimo) `contracts/taxonomy`，含类目树、文件、slug、顺序、`planned` 与依据表。
2. 类目契约独立版本，使用 `CHANGELOG.md` 记录变更。
3. 工具仓库 vendor 副本并校验哈希；[词合 CiHe](https://github.com/chinese-ci/cihe) 从副本读取 slug 与顺序，不再硬编码。
4. 来源类目不进入该契约；来源层自行映射到物料类目候选。
5. 类目增删、路径或顺序变化按契约变更处理：RFC/ADR、版本升级、重新验收。

## 后果

- 类目变更可审计、可复现，物料与方案包顺序一致。
- 工具仓库多一份 vendored 数据，但换来单一事实源。
- 独立类目仓库暂不建立；当出现独立维护团队与发布节奏时再拆。
