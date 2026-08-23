# 代码目前精确停在「法人阶段」

现状与 roadmap 第一阶段吻合：**看清 + 推进可用，记忆自持，周会能读系统——但「提炼」未落地**。数据结构里没有任何「人」的字段，正是阶段（非终态）的证据。

## 现状（设计 ↔ 代码何以对应）

- **清单=次级法人**：`data/tasks.json` 顶层 `{lists:[{id,name,tasks:[...]}]}`，与 Go `List{ID,Name,Tasks}`、Dart `TaskList{id,name,tasks}` 完全一致。`task` = 法人要记住的一件事（id/title/description/status/priority/category）。
- **看清=状态泳道**：`TaskStatus` 枚举 notStarted/inProgress/reviewing/done，与 provider 常量一一对应；看板按状态分列。
- **推进=拖拽即存**：`Task.moveTo`（`copyWith`）→ `PUT /api/lists/{id}/tasks/{taskId}`（upsert）→ 落盘。无「改状态+再记一条」的中间步。
- **记忆自持**：`store.Store`（local/OSS）抽象，`QTCLOUD_EXECUTE_STORE=oss` 走 `qtcloud-execute-provider` 桶；文件也在 git（`data/profile`）可版本化。
- **周会读系统**：客户端 `loadLists()` → 网关 `api.quanttide.com/qtcloud-execute` → FC → OSS，能读全局法人记忆。

## 差距

- **提炼未落地**：`category` 自由字符串、CLI `--json`、仓储抽象均为 AI 归集留了路，但**无运行功能**。对应 roadmap「提炼是北极星、标规划中」。

## 迁移

- 周会「检查每周进度」= 打开客户端读全局清单/看板——这一步在代码上已成立，**只差把看板当周会入口**。
- 下一步是「提炼」：从执行日志蒸馏、先归属次级法人、再写入，人确认。

## 边界

- 这是**当前状态**，不是终态。没有「人」字段不代表永远没有，而是阶段（先法人、后自然人）。
- 「看清+推进可用」≠「产品完成」；它只是 roadmap 第一阶段的验证底座。
