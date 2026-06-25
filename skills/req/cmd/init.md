# /req init <id> <名称>

**执行**：
1. 在 `~/.claude/requirements/<id>-<名称>/` 创建目录
2. 创建 `_state.md`：

```
需求：<名称> | ID：<id> | 创建：<日期>
背景摘要：（待填写）
选定方案：（未选定）
关键约束：（待填写）

进度：
[ ] Step1-背景  [ ] Step2-变更  [ ] Step3-调研
[ ] Step4-方案  [ ] Step5-流程图 [ ] Step6-类图
[ ] Step7-开发  [ ] Step8-评审  [ ] Step9-上线

上次停留：初始化完成，待开始 Step 1
```

3. 输出路径并记住：「已初始化 [<id>] <名称>，路径：~/.claude/requirements/<id>-<名称>/」
4. 本次对话后续命令将操作此路径
5. 提示：「执行 `/req background` 开始第一步」
