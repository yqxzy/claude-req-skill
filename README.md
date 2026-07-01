# claude-req-skill

> Claude Code 需求开发全流程管理 Skill —— 按步骤触发，自动管理上下文，避免单对话过长导致质量下降。

## 解决的问题

需求开发时，一个对话里混杂了背景梳理、流程图、类图、代码调研、方案设计……上下文越来越长，生成质量下降。迭代时也无法恢复上次的上下文。

**核心思路**：每个开发阶段产出物保存为结构化文件，下次对话只加载当前阶段需要的文件，而不是重放整个历史对话。

## 功能概览

10 个步骤，每步由用户手动触发：

```
/req init REQ-001 购物车退款    # 初始化需求

/req background    # Step 1:  梳理背景信息
/req changes       # Step 2:  整理变更点（只记录业务行为，不涉及技术实现）
/req codebase      # Step 3:  调研现有代码
/req solutions     # Step 4:  多方案设计与评估（你来选定）
/req testcases     # Step 5:  生成系统测试核心 Case 表格
/req flowchart     # Step 6:  画执行流程图（PlantUML，box 按服务划分）
/req classdiagram  # Step 7:  画核心类图（蓝=修改类 / 绿=新增类）
/req dev           # Step 8:  开发辅助 + 记录关键决策
/req review        # Step 9:  代码评审
/req release       # Step 10: 上线记录

/req status        # 查看当前需求进度
/req list          # 列出所有需求
/req load REQ-001  # 新对话中恢复上下文
```

## 变更联动更新

任意步骤中，只要消息包含变更信号词，自动触发联动更新，无需手动执行命令：

| 触发词示例 | 说明 |
|-----------|------|
| 「新增一个变更点」「追加一条变更」 | 新增功能/行为 |
| 「把 X 改为 Y」「X 变更为 Y」 | 修改已有行为 |
| 「删除 X 变更」「取消这个功能」 | 删除功能 |

触发后自动执行：
1. 更新 `01_changes.md`
2. 评估各已完成步骤的影响范围
3. 自动更新受影响的 testcases / flowchart / classdiagram
4. 方案设计 / 代码调研标注需人工复查，不自动修改
5. 输出更新报告

## 文件结构

```
~/.claude/skills/req/
  SKILL.md              # 薄路由 + 变更探测触发器
  cmd/                  # 每个命令独立文件，按需加载
    load.md             # ← 跨对话恢复的入口（每次新对话先执行）
    init.md  status.md  list.md
    background.md  changes.md   codebase.md   solutions.md
    testcases.md   flowchart.md classdiagram.md
    dev.md  review.md  release.md
    propagate.md        # ← 变更联动更新逻辑

~/.claude/requirements/
  _specs/
    flowchart-spec.md      # PlantUML 时序图规范（box 按服务划分）
    classdiagram-spec.md   # PlantUML 类图规范（蓝=修改/绿=新增）
  {id}-{name}/             # 每个需求一个目录
    _state.md              # 进度快照（跨对话恢复的数据源）
    00_background.md       # Step 1 产出
    01_changes.md          # Step 2 产出
    02_codebase.md         # Step 3 产出
    03_solutions.md        # Step 4 产出
    04_testcases.md        # Step 5 产出
    05_flowchart.puml      # Step 6 产出
    06_classdiagram.puml   # Step 7 产出
    07_dev_notes.md        # Step 8 产出
    08_review.md           # Step 9 产出
    09_release.md          # Step 10 产出
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

不同对话可以处理同一需求的不同阶段，多需求可同时并行：

```
对话 A（调研阶段）：
  /req init REQ-001 购物车退款
  /req background → /req changes → /req codebase

对话 B（设计阶段）：
  /req load REQ-001    ← 新对话先执行，6 行简报恢复上下文
  /req solutions → /req testcases → /req flowchart → /req classdiagram

对话 C（开发阶段）：
  /req load REQ-001
  /req dev

对话 D（另一个需求）：
  /req load REQ-002    ← 各对话独立，互不干扰
  /req background
```

每个对话通过 `/req load <id>` 独立绑定到对应需求，不共享任何全局状态，多需求并行不串数据。

## 设计原则

- **薄路由**：`SKILL.md` 只做命令分发和变更探测，每次调用只加载对应 cmd 文件
- **最小加载**：每个 cmd 文件只声明该步骤必需的文件，不加载无关内容
- **`_state.md` 是检查点**：进度快照，跨对话恢复只需读它，不重放历史对话
- **对话内存隔离**：当前需求路径存在对话记忆中，不依赖共享文件，多需求并行不互扰
- **先写再改**：每步完成后直接写文件，展示结果后等修改意见，不反复确认
- **变更自动联动**：检测到需求变更时，主动评估并更新已完成的工作项

## 版本历史

**v0.3.0** — 4 项使用体验优化：changes 去技术实现、flowchart box 按服务划分、先写再改、变更联动更新

**v0.2.0** — 薄路由重构：SKILL.md 从 474 行精简，命令拆到 cmd/ 目录；对话内存替代 _active 文件，支持多需求并行

**v0.1.1** — 流程图和类图改为 PlantUML；新增 Step5 系统测试 Case

**v0.1.0** — 初版，10 步需求开发流程
