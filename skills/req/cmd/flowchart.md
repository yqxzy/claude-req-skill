# /req flowchart — Step 6：执行流程图

**加载文件**：
- `_state.md`（确认选定方案）
- `03_solutions.md`（只读「选定方案」的实现路径部分）
- `~/.claude/requirements/_specs/flowchart-spec.md`

**执行**：
1. 从 `_state.md` 确认选定方案，从 `03_solutions.md` 读取其实现路径
2. 严格按 `flowchart-spec.md` 绘制 PlantUML 时序图
3. 以 `@startuml ... @enduml` 代码块预览
4. 等待用户反馈，可多轮修改
5. 每轮确认后立即写入 `05_flowchart.puml`（不等「完全满意」再保存）
6. 更新 `_state.md`：Step6 打勾，上次停留更新

**注意**：`03_solutions.md` 文件可能较长，只需读取选定方案的「实现路径」部分，不需要加载其他方案内容。
