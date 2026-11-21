# Issue tracker

## 公开仓库

公开设计、契约、来源提案与工具 bug 使用 GitHub Issues：

- 词铸 CiZhu：架构、流程、来源提案、物料修正、RFC。
- 词模 CiMo：契约字段、schema、版本与兼容性。
- 词修 CiXiu / 词合 CiHe：工具 bug、测试、文档。
- `chinese-dict` / `rime-wubi`：产物问题。

标签使用 `triage-labels.md` 中的五个角色字符串。

## 私有来源层

平台相关实现、原始文件、来源映射与策展覆盖使用私有仓库词采 CiCai 的本地 Markdown tracker：

- 每个功能一个目录：`../CiCai/.scratch/<feature-slug>/`
- 规格：`../CiCai/.scratch/<feature-slug>/spec.md`
- 工单：`../CiCai/.scratch/<feature-slug>/issues/<NN>-<slug>.md`
- `Status:` 行与评论格式沿用原约定。

公开 Issue 不粘贴平台名称、地址、原始文件或来源类目细节；只能描述平台无关的问题与期望行为。

## Wayfinding

私有来源层的探索仍使用 `.scratch/<effort>/map.md` 与子工单；公开侧用 GitHub Issue 的 `RFC:` 前缀与标签表达同一类决策。
