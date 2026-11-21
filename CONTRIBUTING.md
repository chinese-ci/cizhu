# 贡献指南

先读 [`ARCHITECTURE.md`](ARCHITECTURE.md) 与 [`community/RFC.md`](community/RFC.md)。参与即表示遵守 [`community/CODE_OF_CONDUCT.md`](community/CODE_OF_CONDUCT.md)。

## 可以贡献什么

| 入口 | 位置 | 说明 |
| --- | --- | --- |
| 来源提案 | 词铸 CiZhu Issue | 提出新的文本来源：公开地址、许可/条款、样例文本、期望类目。维护者在私有来源层评估接入。 |
| 自带语料（BYO corpus） | [词修 CiXiu](https://github.com/chinese-ci/cixiu) Issue / PR | 按来源语料契约提供 `sources.json` + `text/`，直接运行词修 CiXiu，不需要私有采集器。 |
| 物料修正 | Issue | 说明词条、期望类目、依据；维护者审核后通过增量语料或私有策展覆盖处理。生成物料不直接编辑。 |
| 契约变更 | [词模 CiMo](https://github.com/chinese-ci/cimo) Issue/PR + RFC | 修改 source-corpus、material、taxonomy 或 target 契约。 |
| target 适配器 | [词合 CiHe](https://github.com/chinese-ci/cihe) PR | 按 `targets/adapter` 契约新增输入法目标；v1 只验收 `rime-wubi`。 |
| 工具代码 | [词修 CiXiu](https://github.com/chinese-ci/cixiu) / [词合 CiHe](https://github.com/chinese-ci/cihe) PR | 清洗逻辑、性能、测试、文档。 |

不接收：原始平台文件、整份来源语料、平台账号/凭据、绕过平台访问限制的实现。

## 自带语料

最小示例在 [`cixiu/examples/minimal-corpus`](https://github.com/chinese-ci/cixiu)。结构：

```text
corpus/
├── sources.json
└── text/
    └── <source_id>.txt
```

`sources.json` 的 `schema_version` 为 1；每个来源包含 `source_id`、`order`、`categories`（物料类目候选）。契约正本见[词模 CiMo](https://github.com/chinese-ci/cimo) `contracts/source-corpus.md`。

运行：

```bash
cd cixiu
uv run python tools/run_cixiu.py --corpus ../corpus --material-dir var/material
```

想产出更小的词库：缩小语料范围即可；也可以在[词合 CiHe](https://github.com/chinese-ci/cihe) 用 `import_tables` 裁剪类目。v1 不提供按类目筛选语料的额外 profile。

## 物料修正

物料是生成物，直接改 `chinese-dict` 会在下一次重建时丢失。正确流程：

1. 在词铸 CiZhu 提 Issue，写清词条、当前类目、期望类目/删除理由与可核验依据。
2. 维护者审核后，以增量语料或私有策展覆盖进入管线。
3. 重新生成物料，走完整验收。

增量语料的语义是**只追加新词**；删除与改类目由维护者在来源层处理，v1 不提供公开的删除操作。

## 契约与 target 变更

- 契约变更先提 RFC，再改[词模 CiMo](https://github.com/chinese-ci/cimo)；工具仓库更新 vendored 副本与哈希。
- 新增 target 适配器：在[词合 CiHe](https://github.com/chinese-ci/cihe) `src/cihe/targets/<name>/` 实现 encode/emit/verify，附契约测试与示例；默认 target 不变。
- 跨仓库链接在公开文档中使用完整 GitHub URL；仓库内使用相对链接。

## 开发环境

- Python 3.13、[uv](https://docs.astral.sh/uv/)、Docker（[词合 CiHe](https://github.com/chinese-ci/cihe) 的编码器镜像）。
- 每个工具仓库独立同步依赖：`uv sync`。
- 提交前：`uv run pytest`、`uv run pyright`。
- 文档改动：检查 Markdown 链接与中英文术语一致性。

## 工单与评审

- 公开设计、契约、来源提案：GitHub Issues（词铸 CiZhu / 词模 CiMo）。
- 工具 bug 与实现：对应工具仓库 Issues。
- 平台相关实现与数据问题：私有来源层处理，不在公开 Issue 中粘贴来源细节。
- PR 描述写清动机、契约影响、测试与物料/产物是否变化。

## 许可

- 文档：[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)，见 `LICENSE`。
- 文档内代码片段：[MIT](https://opensource.org/license/mit)，见 `LICENSE-MIT`。
- 物料：[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/)；默认目标方案包：[LGPL-3.0](https://www.gnu.org/licenses/lgpl-3.0.html)。
