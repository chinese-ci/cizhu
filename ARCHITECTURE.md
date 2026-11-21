# 架构

## 目标

- 同一份来源语料，重复运行得到逐字节一致的纯词库物料。
- 物料只含词条字符串，不含编码、频次、权重、来源或原始字形。
- 采集、清洗、构建三个工具通过稳定契约连接，可独立替换与测试。
- 默认交付 [Rime](https://github.com/rime/rime) 五笔 86 方案包，同时保留扩展到其它输入法目标的接口。

## 非目标

- 不提供来源平台本身的访问能力；平台适配属于私有的来源层。
- 不在公开仓库收录具体来源名称、地址、平台类目体系或原始文件。
- v1 不实现默认目标之外的输入法产物；只保留 target 适配器契约。

## 三段式管道

### 1. 来源层（词采 CiCai，私有）

- 枚举来源、下载原始文件、解析格式、库级筛选。
- 负责把来源元数据映射为物料类目候选。
- 为每个来源分配稳定、不可反推的 `source_id`，并私下保存 `source_id` 到真实来源的关联表。
- 输出来源语料：`sources.json` + `text/<source_id>.txt`。
- 来源类目、平台地址、文件后缀、限速与反封禁策略只存在于这一层。

### 2. 词修 CiXiu 层（词修 CiXiu，公开工具）

- 只读取来源语料契约，不解析任何平台私有格式。
- 条级清洗、字形归一、去重、类目候选裁决。
- 输出纯词库物料与私有清洗报告。
- 不处理“删除词条”或“整库排除”；这些属于来源层的策展职责。

### 3. 构建层（词合 CiHe，公开工具）

- 只读取纯词库物料契约。
- 通过 target 适配器完成编码与 Ci 产物生成；默认 target 为 `rime-wubi`，只输出 Ci 类目词表。
- 输入法项目的基础结构（schema、prism/OpenCC 配置、字表、主表、README/LICENSE）由产物仓库维护，[词合 CiHe](https://github.com/chinese-ci/cihe) 不生成也不修改。
- 默认目标的产物在 `rime-wubi` 组织下：`rime-wubi/zi` 构建字表，`rime-wubi/rime-wubi` 维护基础项目并组装、发布方案包。
- 其它 target 必须显式指定。

## 契约

契约正本在[词模 CiMo](https://github.com/chinese-ci/cimo)，工具仓库以 vendored 副本消费并校验版本与哈希。

| 契约 | 生产方 | 消费方 |
| --- | --- | --- |
| `source-corpus` | 来源层 / BYO 生产者 | [词修 CiXiu](https://github.com/chinese-ci/cixiu) |
| `material` | [词修 CiXiu](https://github.com/chinese-ci/cixiu) | [词合 CiHe](https://github.com/chinese-ci/cihe)、物料仓库 |
| `taxonomy` | 类目维护者 | [词修 CiXiu](https://github.com/chinese-ci/cixiu)、[词合 CiHe](https://github.com/chinese-ci/cihe)、验收、物料仓库 |
| `targets/adapter` | [词合 CiHe](https://github.com/chinese-ci/cihe) | 各 target 适配器 |
| `targets/rime-wubi` | [词合 CiHe](https://github.com/chinese-ci/cihe)（Ci 词表）、[rime-wubi](https://github.com/rime-wubi/rime-wubi) 基础项目 | `rime-wubi` 产物仓库 |

## 来源标识与可观测性

- `source_id` 是唯一关联键：不透明、稳定、跨运行不变。
- [词修 CiXiu](https://github.com/chinese-ci/cixiu) 的报告与事件只记录 `source_id`；不记录标题、描述、平台或库 ID。
- 真实来源上下文由生产者的私有映射表保存；需要排查时用 `source_id` 回查。
- 增量来源作为新的 `source_id` 进入管线；去重与类目裁决按契约规则参与，不因“增量”获得特殊地位。

## 类目

- **来源类目**：生产者私有，只用于把来源库映射到物料类目。
- **物料类目**：工程契约，依据公开标准与调研确定，包含类目树、文件、slug、顺序与 `planned` 标记。
- 物料类目的增删与顺序变更属于契约变更，必须走 RFC/ADR 与版本升级。

## 确定性

同一输入必须得到相同物料。顺序规则：

1. 来源 `order` 升序；
2. 同序按 `source_id` 字典序；
3. 同来源内按文本行序；
4. 类目与文件顺序由 taxonomy 契约固定；
5. 展示形与类目裁决按契约规定的稳定键，不使用哈希随机或文件系统遍历顺序。

## 公开与私有

- 公开：词铸 CiZhu、[词模 CiMo](https://github.com/chinese-ci/cimo)、[词修 CiXiu](https://github.com/chinese-ci/cixiu)、[词合 CiHe](https://github.com/chinese-ci/cihe)、`chinese-dict`、默认目标产物仓库。
- 私有：词采 CiCai，以及其中的平台适配、原始文件、来源映射、策展覆盖。
- 公开侧只保留平台无关的结论、契约、物料与验收摘要。
