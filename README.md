# claude-req-skill

> Claude Code 需求开发全流程管理 Skill —— 按步骤触发，自动管理上下文，避免单对话过长导致质量下降。

## 解决的问题

需求开发时，一个对话里混杂了背景梳理、流程图、类图、代码调研、方案设计……上下文越来越长，生成质量下降。迭代时也无法恢复上次的上下文。

**核心思路**：每个开发阶段产出物保存为结构化文件，下次对话只加载当前阶段需要的文件，而不是重放整个历史对话。

## 功能概览

9 个步骤，每步由用户手动触发：

```
/req init REQ-001 购物车退款    # 初始化需求

/req background    # Step 1: 梳理背景信息
/req changes       # Step 2: 整理变更点
/req codebase      # Step 3: 调研现有代码
/req solutions     # Step 4: 多方案设计与评估（你来选定）
/req flowchart     # Step 5: 画执行流程图（可多轮修改）
/req classdiagram  # Step 6: 画核心类图（可多轮修改）
/req dev           # Step 7: 开发辅助 + 记录关键决策
/req review        # Step 8: 代码评审
/req release       # Step 9: 上线记录

/req status        # 查看当前需求进度
/req list          # 列出所有需求
/req load REQ-001  # 新对话中恢复上下文
```

## 文件结构

```
~/.claude/skills/req/
  SKILL.md              # 薄路由（23行），只做命令分发
  cmd/                  # 每个命令独立文件（14～27行）
    load.md             # ← 跨对话恢复的入口
    background.md  changes.md  codebase.md  solutions.md
    flowchart.md   classdiagram.md  dev.md  review.md  release.md
    init.md  status.md  list.md

~/.claude/requirements/
  _active               # 当前激活需求的路径（跨对话切换靠它）
  _specs/
    flowchart-spec.md      # PlantUML 时序图规范
    classdiagram-spec.md   # PlantUML 类图规范（蓝=修改/绿=新增）
  {id}-{name}/          # 每个需求一个目录
    _state.md           # 进度快照（跨对话恢复的数据源）
    00_background.md  01_changes.md  02_codebase.md
    03_solutions.md   04_flowchart.puml  05_classdiagram.puml
    06_dev_notes.md   07_review.md  08_release.md
```

## 安装

```bash
# 1. clone 仓库
git clone https://github.com/yqxzy/claude-req-skill.git

# 2. 复制 skill 文件到 Claude Code 全局目录
cp -r claude-req-skill/skills/req ~/.claude/skills/

# 3. 复制规范文件
mkdir -p ~/.claude/requirements/_specs
cp claude-req-skill/requirements/_specs/* ~/.claude/requirements/_specs/
```

## 跨对话使用

不同对话可以处理同一需求的不同阶段：

```
对话 A：/req init REQ-001 购物车退款
        /req background → /req changes → /req codebase
        （对话结束，进度自动保存在 _state.md）

对话 B：/req load REQ-001    ← 先恢复上下文
        /req solutions
        /req flowchart
        /req classdiagram

对话 C：/req load REQ-001    ← 先恢复上下文
        /req dev
```

每次新对话开始先执行 `/req load <id>`，读取 `_state.md` 中的进度快照，6 行简报即可恢复上下文。

## 设计原则

- **薄路由**：`SKILL.md` 只有 23 行，每次调用只加载当前命令对应的 cmd 文件（14～27 行）
- **最小加载**：每个 cmd 文件只声明该步骤必需的文件，不加载无关内容
- **`_state.md` 是检查点**：存储进度快照，跨对话恢复只需读它，不重放历史对话
- **用户触发，不自动推进**：每步由你主动发起，完成后等待下一指令

## 版本

**v0.2.0** — 薄路由重构：SKILL.md 从 474 行精简为 23 行，命令拆分到 cmd/ 目录按需加载；支持跨对话场景。
