# 词铸 CiZhu

中文词条的采集、清洗、物料化与输入法产物构建工程。公开侧只处理平台无关的契约、文本与物料；具体来源细节留在私有来源层。

## 语言

**词铸 CiZhu**：
由来源层、[词修 CiXiu](https://github.com/chinese-ci/cixiu)、[词合 CiHe](https://github.com/chinese-ci/cihe) 三个阶段组成的工程；本仓库是工程文档与仓库地图。
_Avoid_: 词库工程

**来源层（词采 CiCai）**：
私有阶段：枚举来源、下载原始文件、解析格式、库级筛选、映射物料类目，并输出来源语料。
_Avoid_: 爬虫、采集器

**[词修 CiXiu](https://github.com/chinese-ci/cixiu)**：
只读取来源语料，执行条级清洗、字形归一、去重与类目候选裁决，输出纯词库物料。
_Avoid_: 清洗器

**[词合 CiHe](https://github.com/chinese-ci/cihe)**：
读取纯词库物料，通过 target 适配器编码并生成目标项目的 Ci 内容片段；默认 target 是 `rime-wubi`，只输出类目词表。输入法项目的基础结构由产物仓库维护。
_Avoid_: Forge、转换器

**来源语料**：
来源层或 BYO 生产者按 `source-corpus` 契约输出的文本与元数据：`sources.json` + `text/<source_id>.txt`。
_Avoid_: 原始词库、抓取结果

**来源标识 (source_id)**：
不透明、稳定、跨运行不变的来源键；[词修 CiXiu](https://github.com/chinese-ci/cixiu) 只用它关联报告，不解析其含义。
_Avoid_: 平台 ID、库 ID

**来源类目**：
来源层私有的类目体系；只用于把来源库映射为物料类目，不进入公开契约。
_Avoid_: 平台类目

**物料类目**：
工程契约中的类目树、文件、slug 与顺序；依据公开标准与调研确定，正本在[词模 CiMo](https://github.com/chinese-ci/cimo) `taxonomy`。
_Avoid_: 分类、目录

**纯词库物料**：
`chinese-dict` 发布的、UTF-8 一行一个词条、不含编码/频次/权重/来源的词条集合。
_Avoid_: 词库、物料库

**词条**：
物料或语料中的一行，一个单字或一个词语。
_Avoid_: 条目、词

**target**：
[词合 CiHe](https://github.com/chinese-ci/cihe) 的输出目标；一个 target 定义编码、产物文件与校验方式。默认 target 为 `rime-wubi`。
_Avoid_: 格式、后端

**target 适配器**：
实现 `targets/adapter` 契约的模块，负责 encode/emit/verify。
_Avoid_: 转换器插件

**BYO corpus**：
自带语料；任何满足 `source-corpus` 契约的生产者都可以直接使用词修 CiXiu，不依赖私有来源层。
_Avoid_: 外部词库

**[Rime](https://github.com/rime/rime) 词库**：
`rime-wubi` 方案使用的、带五笔编码的词典文件。
_Avoid_: 词库

**字表**：
Rime 方案中几乎不变的单字到五笔编码的映射。
_Avoid_: 单字集

**词表**：
Rime 方案中由类目词条组成的、可独立维护的词语集合。
_Avoid_: 词库

**[chinese-dict](https://github.com/chinese-ci/chinese-dict)**：
`chinese-ci` 组织下发布纯词库物料的仓库。

**[rime-wubi](https://github.com/rime-wubi/rime-wubi)**：
发布默认 Rime 五笔 86 方案包的 GitHub 组织（`rime-wubi/rime-wubi`）。

单独使用「词库」不指向确定概念；按上下文写作来源语料、纯词库物料或 Rime 词库。
