# req - 需求开发助手

收到 `/req <命令> [参数]` 时：

1. 若命令不是 `init` / `load` / `list`，先读取 `~/.claude/requirements/_active` 获取当前需求路径
   - 文件不存在则提示：「当前无激活需求，请先执行 `/req load <id>` 或 `/req init <id> <名称>`」
2. 读取对应的命令文件，按其指令执行

| 命令 | 功能 | 指令文件 |
|------|------|---------|
| `init <id> <名称>` | 初始化新需求 | `~/.claude/skills/req/cmd/init.md` |
| `load <id>` | 跨对话恢复上下文 ← 新对话首先执行 | `~/.claude/skills/req/cmd/load.md` |
| `list` | 列出所有需求及进度 | `~/.claude/skills/req/cmd/list.md` |
| `status` | 查看当前需求进度 | `~/.claude/skills/req/cmd/status.md` |
| `background` | Step 1：梳理背景信息 | `~/.claude/skills/req/cmd/background.md` |
| `changes` | Step 2：整理变更点 | `~/.claude/skills/req/cmd/changes.md` |
| `codebase` | Step 3：调研现有代码 | `~/.claude/skills/req/cmd/codebase.md` |
| `solutions` | Step 4：多方案设计与评估 | `~/.claude/skills/req/cmd/solutions.md` |
| `flowchart` | Step 5：绘制执行流程图 | `~/.claude/skills/req/cmd/flowchart.md` |
| `classdiagram` | Step 6：绘制核心类图 | `~/.claude/skills/req/cmd/classdiagram.md` |
| `dev` | Step 7：开发辅助模式 | `~/.claude/skills/req/cmd/dev.md` |
| `review` | Step 8：代码评审 | `~/.claude/skills/req/cmd/review.md` |
| `release` | Step 9：上线记录 | `~/.claude/skills/req/cmd/release.md` |
