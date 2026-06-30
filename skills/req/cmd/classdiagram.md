# /req classdiagram — Step 7：核心类图

**加载文件**：
- `_state.md`（确认选定方案和关键约束）
- `05_flowchart.puml`（了解数据流和参与者）
- `02_codebase.md`（现有类结构参考，只读「核心类/接口位置」部分）
- `~/.claude/requirements/_specs/classdiagram-spec.md`

**执行**：
1. 从流程图识别参与的核心类，结合代码调研确认现有类结构
2. 确认绘制范围：只画 Repository 层及以上（新增类 / 修改类 / 被依赖的未变动类）
3. 严格按 `classdiagram-spec.md` 绘制 PlantUML 类图：
   - 修改类：`#lightblue`
   - 新增类：`#lightgreen`
   - 未变动类：无色
   - 新增方法加 `//` 注释
4. 预览后等待反馈，多轮修改，每轮确认后写入 `06_classdiagram.puml`
5. 图后附「变更说明」文字
6. 更新 `_state.md`：Step7 打勾，上次停留更新
