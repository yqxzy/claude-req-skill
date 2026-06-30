# /req review — Step 9：代码评审

**加载文件**：
- `_state.md`（选定方案名、关键约束）
- `05_flowchart.puml`（验证代码流程是否符合设计）
- `06_classdiagram.puml`（验证类设计是否符合设计）
- `07_dev_notes.md`（了解开发过程中的偏差背景）

**执行**：
1. 展示设计基准摘要（方案名 + 流程图参与者 + 类图变更说明），共 5 行以内
2. 调用 `/code-review` 进行代码质量检查
3. 在代码质量之外，额外核查：
   - 代码流程是否与 `05_flowchart.puml` 一致
   - 类结构是否与 `06_classdiagram.puml` 吻合
   - `07_dev_notes.md` 中记录的偏差是否有合理解释
4. 将评审结论写入 `08_review.md`：通过 / 有问题（列问题清单）
5. 更新 `_state.md`：Step9 打勾，上次停留更新
