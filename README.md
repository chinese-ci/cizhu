# 词铸 CiZhu

把中文词条从来源语料加工为可发布、可复现、结构与顺序稳定的输入法词库物料，并构建为目标输入法产物。工程按“契约先行、工具分离”组织：文档与契约在词铸 CiZhu 与[词模 CiMo](https://github.com/chinese-ci/cimo)，采集、清洗、构建各自独立。

```text
来源语料 ──▶ 词修 CiXiu（清洗 / 字形归一 / 去重 / 归类）──▶ 纯词库物料 ──▶ 词合 CiHe（编码 / Ci 词表）──▶ rime-wubi（基础项目 / 组装）──▶ 输入法产物
    ▲                                                                     │
    └──────────────────── 词采 CiCai（私有来源层）◀─────────────────────────────┘
```

## 仓库地图

| 仓库 | 可见性 | 职责 |
| --- | --- | --- |
| [chinese-ci/cizhu](https://github.com/chinese-ci/cizhu) | 公开 | 工程设计、仓库地图、社区参与、规格与决策 |
| [chinese-ci/cimo](https://github.com/chinese-ci/cimo) | 公开 | 跨工具契约：来源语料、物料、类目、目标适配器 |
| [chinese-ci/cixiu](https://github.com/chinese-ci/cixiu) | 公开 | 清洗工具：文本语料 → 纯词库物料 |
| [chinese-ci/cihe](https://github.com/chinese-ci/cihe) | 公开 | 构建工具：物料 → [rime-wubi](https://github.com/rime-wubi/rime-wubi) Ci 类目词表 |
| `chinese-ci/cicai` | 私有 | 来源层：平台适配、下载、解析、库级筛选与来源关联 |
| [chinese-ci/chinese-dict](https://github.com/chinese-ci/chinese-dict) | 公开 | 纯词库物料发布 |
| [rime-wubi/rime-wubi](https://github.com/rime-wubi/rime-wubi) | 公开 | [Rime](https://github.com/rime/rime) 五笔 86 基础项目与方案包发布 |
| [rime-wubi/zi](https://github.com/rime-wubi/zi) | 公开 | 字表构建：[国标 8,105 单字](https://www.gov.cn/zhengce/zhengceku/2013-08/19/content_1289.htm) + 表外增广 |

平台与来源细节只在私有的词采 CiCai；公开仓库不包含具体来源名称、地址或平台类目体系。

## 快速开始

自带语料（任意文本来源）：

```bash
# 1. 准备 sources.json 与 text/<source_id>.txt，见 CIMO 的来源语料契约
# 2. 清洗为物料
cd cixiu
uv run python tools/run_cixiu.py --corpus ../my-corpus --material-dir var/material

# 3. 构建 rime-wubi 的 Ci 类目词表
cd ../cihe
uv run python tools/run_consumer.py --material ../cixiu/var/material \
  --char-table ../rime-wubi/wubi86.zi.dict.yaml

# 4. 把 ci/ 与 rime-wubi 基础项目组装成方案包，由 rime-wubi 发布流程完成
```

维护者完整管线由私有的词采 CiCai 产出语料，再走[词修 CiXiu](https://github.com/chinese-ci/cixiu) → [词合 CiHe](https://github.com/chinese-ci/cihe) → [rime-wubi](https://github.com/rime-wubi/rime-wubi) 组装；验收记录在 `releases/`。

## 文档

- [`ARCHITECTURE.md`](ARCHITECTURE.md)：管道阶段、契约边界、数据流与非目标
- [`spec/`](spec/)：v1 产品需求与验收口径
- [`decisions/`](decisions/)：ADR
- [`glossary/GLOSSARY.md`](glossary/GLOSSARY.md)：术语
- [`releases/`](releases/)：版本矩阵与发布记录
- [`community/`](community/)：社区参与、RFC 与行为准则
- [`research/`](research/)：标准与无平台调研
- 契约正本：[`chinese-ci/cimo`](https://github.com/chinese-ci/cimo)

## 状态

第一阶段（v1）已完成一次全量运行与验收：物料 11,325,581 条、73 个类目文件，默认目标方案包通过真实部署查码。工程仓库拆分后的工具链在全量数据上重跑，物料与方案包内容逐字节不变，验收 89/89。契约尚未正式发布，仍在第一阶段内调整；变更记入各契约的 `CHANGELOG.md`。
