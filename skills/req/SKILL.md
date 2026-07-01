# req - 需求开发助手

## 变更探测（优先于命令路由执行）

当用户消息中出现以下词语，且本次对话已执行过 `/req load`：
- 「新增变更」「追加变更」「新增一个」「追加一条」
- 「改为」「变更为」「修改成」「调整为」
- 「删除变更」「去掉这个」「取消这个功能」

→ 不等用户执行 `/req` 命令，自动读取并执行 `~/.claude/skills/req/cmd/propagate.md`

---

收到 `/req <命令> [参数]` 时：

1. **确定当前需求路径**（`init` / `load` / `list` 命令跳过此步）：
   - 若本次对话已执行过 `/req load`，使用那次确定的需求路径
   - 若本次对话尚未执行 `/req load`，提示用户：
     「请先执行 `/req load <id>` 加载需求，或 `/req list` 查看所有需求」
   - **不读取任何共享文件来判断当前需求**，防止多需求并行时互相干扰

2. 读取对应命令文件，按其指令执行：

| 命令 | 功能 | 指令文件 |
|------|------|---------|
| `init <id> <名称>` | 初始化新需求 | `~/.claude/skills/req/cmd/init.md` |
| `load <id>` | 跨对话恢复上下文 ← 每次新对话先执行 | `~/.claude/skills/req/cmd/load.md` |
| `list` | 列出所有需求及进度 | `~/.claude/skills/req/cmd/list.md` |
| `status` | 查看当前需求进度 | `~/.claude/skills/req/cmd/status.md` |
| `background` | Step 1：梳理背景信息 | `~/.claude/skills/req/cmd/background.md` |
| `changes` | Step 2：整理变更点 | `~/.claude/skills/req/cmd/changes.md` |
| `codebase` | Step 3：调研现有代码 | `~/.claude/skills/req/cmd/codebase.md` |
| `solutions` | Step 4：多方案设计与评估 | `~/.claude/skills/req/cmd/solutions.md` |
| `testcases` | Step 5：系统测试核心 Case | `~/.claude/skills/req/cmd/testcases.md` |
| `flowchart` | Step 6：绘制执行流程图 | `~/.claude/skills/req/cmd/flowchart.md` |
| `classdiagram` | Step 7：绘制核心类图 | `~/.claude/skills/req/cmd/classdiagram.md` |
| `dev` | Step 8：开发辅助模式 | `~/.claude/skills/req/cmd/dev.md` |
| `review` | Step 9：代码评审 | `~/.claude/skills/req/cmd/review.md` |
| `release` | Step 10：上线记录 | `~/.claude/skills/req/cmd/release.md` |
