---
applyTo: '**'
---

# 工作区记忆体系 — 通用启动协议

> 此指令文件在**所有工作区**自动生效。
> 每次新对话开始时，Agent 必须执行以下检测与启动流程。

---

## 🔍 第一步：检测工作区记忆体系

在回应用户请求之前，**检查当前工作区根目录是否存在 `AGENTS.md`**：

### ✅ 情况A：AGENTS.md 存在（记忆体系已初始化）

按顺序读取这些文件：
1. `AGENTS.md` —— 工作区地图与规则（含 `nblm_brain_id` 长期记忆笔记本ID，若已配置）
2. `MEMORY.md` —— **短期记忆**（项目背景 + Last Session 摘要，若存在）
3. `TODO.md` —— 当前任务清单（若存在）
4. `SOUL.md` —— 人格定义（若存在）

**🧠 检测 NotebookLM 长期记忆**（若 AGENTS.md 含 `nblm_brain_id`）：
- 涉及历史数据、架构查询、跨批次分析时，执行：
  `notebooklm ask "问题" --notebook <nblm_brain_id>`

然后在第一条回复中输出状态报告：
```
---
🤖 记忆已加载 ✅ | 工作区：[工作区名称]
📋 Last Session：[MEMORY.md 中 Last Session 的日期与摘要]
🔥 进行中/高优先任务：[TODO.md 中的关键任务]
🧠 长期记忆：[AGENTS.md 中 nblm_brain_id 若存在则显示，否则显示"未配置"]
---
```

### ⚠️  情况B：AGENTS.md 不存在（未初始化）

在回复最开头提示：
```
---
⚠️  当前工作区尚未初始化记忆体系。
建议运行初始化：我可以为你创建 AGENTS.md / SOUL.md / MEMORY.md / TODO.md / memory/ 目录结构。
是否现在初始化？（输入"是"或直接继续当前任务）
---
```
若用户同意初始化，执行【初始化协议】（见下方）。

---

## 📝 第二步：执行工作

遵循 SOUL.md 中定义的原则执行用户请求。

---

## 🔒 第三步：收尾协议

**每次对话结束时必须执行**（若工作区已初始化记忆体系）：

1. 更新 `TODO.md`：完成的任务改为 `[x]`，新任务追加到对应优先级
2. **执行三级记忆归档（Agent Memory Hierarchy）**，绝对禁止漏操作：
   - 🛡️ **第一级：事实验证层 (Factual Memory)**。如果总结了“用户偏好”、“特定API的不变踩坑规律”等**客观事实**，**必须**调用原生 MCP `memory` 提供图谱工具（调用 `insert` 或 `create`），以增量格式追加保存入 `/memories/` 目录。
   - 📂 **第二级：短期动态跟踪层 (Project Context Memory)**。对于“本项目当前的开发进度”、“刚刚经历的代码逻辑陷阱”等**动态跟踪**，应使用 `claude-mem-global` Skill 向长期 timeline 写入，或在本地 `MEMORY.md` 的 `## Last Session` 下方采用**增量追加**（不可覆盖历史）。
   - 🧠 **第三级：长期知识库层 (Archival Memory - NBLM)**。若 AGENTS.md 含 `nblm_brain_id` 且遇到（修复罕见Bug、完成数据演练、修改表结构、识别跨批次趋势），推送到 NotebookLM。这负责极大量的长期记忆检索：
   ```bash
   notebooklm use <nblm_brain_id>
   notebooklm source add NBLM_PROJECT_BRAIN.md --type file
   ```
3. 按需写入 `memory/daily/`、`memory/decisions/`、`memory/learnings/`
4. 确认："✅ 记忆资产已更新，下次对话将自动加载。"

---

## 🚀 初始化协议（为新工作区创建记忆体系）

当用户同意初始化时，优先推荐使用一键初始化脚本：

```powershell
# 在新工作区根目录执行（PowerShell）
powershell -ExecutionPolicy Bypass -File "C:\Users\34824\AppData\Roaming\Code\User\workspace-template\init-workspace.ps1" -WorkspaceName "项目名" -Language "Python"
```

若手动创建，在**工作区根目录**创建以下文件（模板位于 `C:\Users\34824\AppData\Roaming\Code\User\workspace-template\`）：

```
{工作区根目录}/
├── AGENTS.md       ← 工作区地图 + 启动协议
├── SOUL.md         ← 人格定义（工作原则 + PersonalityState）
├── MEMORY.md       ← 长期记忆（项目背景 + Last Session）
├── TODO.md         ← 任务清单（三优先级）
└── memory/
    ├── daily/      ← 每日工作记录
    ├── decisions/  ← 重要决策日志
    └── learnings/  ← 经验教训
```

同时在 `.vscode/` 目录创建 `agent.instructions.md`。

---

## 📌 优先级规则

- **工作区层** (`.vscode/agent.instructions.md`) 的规则**优先于**本全局规则
- 若工作区有自定义 SOUL.md，以 SOUL.md 中的 PersonalityState 为准
