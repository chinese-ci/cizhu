# 领域文档

工程类 Skill 在探索代码库时如何读取本仓库的领域文档。

## 探索之前先读这些

- [`glossary/GLOSSARY.md`](../glossary/GLOSSARY.md)：跨仓库术语。
- [`decisions/`](../decisions/)：ADR；先读与当前改动相关的决策。
- [`../CiMo/contracts/`](../../CiMo/contracts/)：规范性契约；只在涉及跨工具数据时读取。

上述文件不存在时静默继续，不要提示缺失。

## 文件结构

```
/
├── glossary/GLOSSARY.md
├── decisions/
│   ├── 0001-<decision-slug>.md
│   └── 0002-<decision-slug>.md
├── spec/
└── ../CiMo/contracts/
```

## 使用术语表里的词汇

输出中命名领域概念时（issue 标题、RFC、假设、测试名称），使用术语表定义的词，不要漂移到它明确避免的同义词。新概念先补术语表或 ADR，再写代码。
