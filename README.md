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

每个需求保存在 `~/.claude/requirements/{id}-{name}/`：

```
00_background.md      # 背景信息
01_changes.md         # 变更点
02_codebase.md        # 现有代码调研
03_solutions.md       # 多方案评估
04_flowchart.puml     # 执行流程图（PlantUML 时序图）
05_classdiagram.puml  # 核心类图（PlantUML，蓝=修改/绿=新增）
06_dev_notes.md       # 开发记录（关键决策/坑点）
07_review.md          # 代码评审记录
08_release.md         # 上线记录
_state.md             # 当前进度
```

规范文件（所有需求共享）：

```
~/.claude/requirements/_specs/
  flowchart-spec.md      # 流程图格式规范
  classdiagram-spec.md   # 类图格式规范
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

## 设计原则

- **按需加载**：每步只读取必要文件，例如 `/req flowchart` 只加载背景+方案+流程图规范，不加载代码调研
- **用户触发，不自动推进**：每个步骤由你主动发起，完成后等待下一指令
- **规范与需求分离**：`_specs/` 中的格式规范被所有需求共用，修改规范即全局生效
- **跳步保护**：缺少前置文件时会提示确认，不会静默跳过

## 版本

**v0.1.1** — 流程图改为 PlantUML 时序图，类图改为 PlantUML（蓝=修改/绿=新增，DDD 分层）。

后续计划：`/req codebase`、`/req solutions`、`/req review` 三步改造为并行 Workflow，提升分析质量。
