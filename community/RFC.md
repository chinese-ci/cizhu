# RFC 流程

RFC 用于改动跨仓库契约、架构边界或默认 target。小的文档修正与工具内部重构不需要 RFC。

## 何时需要 RFC

- 修改 `source-corpus`、`material`、`taxonomy` 或 target 契约。
- 新增 target 类型或改变默认 target。
- 改变来源语料的最小字段、顺序规则或去重语义。
- 改变公开/私有边界。

## 步骤

1. 在词铸 CiZhu 提 Issue，标题 `RFC: <主题>`。
2. 按模板填写：动机、现状、提案、契约影响、兼容性、测试与验收、备选方案。
3. 至少一个维护者评审；涉及契约的 RFC 需要[词模 CiMo](https://github.com/chinese-ci/cimo) 维护者确认。
4. 通过后：更新[词模 CiMo](https://github.com/chinese-ci/cimo) 契约与 `CHANGELOG.md`，更新 `releases/matrix.md`，再改工具。
5. 被拒绝或撤回的 RFC 保留在 Issue 中，不删除。

## 模板

```markdown
## 动机
为什么现在要改？不改会怎样？

## 现状
涉及哪些仓库、契约、版本？

## 提案
具体改动；给出字段、文件、顺序规则或接口。

## 契约影响
是否破坏兼容？版本是否升级？是否需要重新验收？

## 测试与验收
如何证明没有意外改变产出？

## 备选方案
考虑过哪些方案，为什么不选？
```
