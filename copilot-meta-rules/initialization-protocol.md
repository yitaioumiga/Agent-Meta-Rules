# 初始化协议

> 本文件定义了为新工作区创建完整记忆体系的操作流程。
> 当工作区根目录不存在 `AGENTS.md` 时，Agent **必须**按照本文件执行初始化。

---

## 1. 触发条件

当 Agent 在启动协议中检测到工作区根目录不存在 `AGENTS.md` 文件时，触发初始化。

---

## 2. 初始化操作

### 2.1 创建核心文件

按以下模板创建核心文件：

#### AGENTS.md

```markdown
# [项目名称] — 工作区信息

## 基本信息
- **项目名称**：[项目名称]
- **项目描述**：[一句话描述]
- **技术栈**：[主要技术栈]
- **创建日期**：[YYYY-MM-DD]

## 工作区目标
- [目标1]
- [目标2]

## 重要链接
- [相关文档链接]

## 元规则体系
- 主文件：`copilot-meta-rules/META-RULES.md`
- 详细规则：`copilot-meta-rules/` 目录下的所有文件
```

#### SOUL.md

```markdown
# [项目名称] — 人格定义

## 工作原则
- [原则1]
- [原则2]

## 价值观
- [价值观1]
- [价值观2]

## 编码风格偏好
- [偏好1]
- [偏好2]

## 禁止事项
- [禁止1]
- [禁止2]
```

#### MEMORY.md

```markdown
# [项目名称] — 短期记忆

## 项目背景
[项目背景描述]

## Last Session
> 下次会话的摘要将追加在这里（增量追加，不覆盖历史）
```

#### TODO.md

```markdown
# [项目名称] — 任务清单

## 🔴 高优先级
- [ ] [任务1]

## 🟡 中优先级
- [ ] [任务2]

## 🟢 低优先级
- [ ] [任务3]
```

### 2.2 创建记忆文件

#### memory/experiences.md

```markdown
# 经验库

> 本文件存储从对话中提取的经验条目。
> 格式见 `copilot-meta-rules/sedimentation-path.md`。

---
```

#### memory/counters.md

```markdown
# 计数器

> 本文件追踪问题、经验、规则的出现频率。
> 格式见 `copilot-meta-rules/counter-mechanism.md`。

---
```

#### memory/facts.md

```markdown
# 事实记忆

> 本文件存储经过验证的客观事实。
> 格式见 `copilot-meta-rules/memory-management.md`。

---
```

#### memory/principles.md

```markdown
# 原则库

> 本文件存储从经验中归纳的通用原则。
> 格式见 `copilot-meta-rules/sedimentation-path.md`。

---
```

### 2.3 创建目录

创建以下空目录（每个目录下放置一个 `.gitkeep` 文件以保留目录结构）：

- `memory/decisions/` — 决策日志
- `memory/learnings/` — 经验教训
- `memory/patterns/` — 成功模式
- `memory/daily/` — 每日工作记录
- `skills/` — Skill 库

---

## 3. 初始化后的目录结构

```
{工作区根目录}/
├── AGENTS.md           ← 工作区地图与规则
├── SOUL.md             ← 人格定义与工作原则
├── MEMORY.md           ← 短期记忆（项目背景 + Last Session）
├── TODO.md             ← 任务清单
├── memory/
│   ├── experiences.md  ← 经验库（含结构化计数）
│   ├── counters.md     ← 计数器（问题/经验/规则频率追踪）
│   ├── facts.md        ← 事实记忆
│   ├── principles.md   ← 原则库
│   ├── decisions/      ← 决策日志
│   ├── learnings/      ← 经验教训
│   ├── patterns/       ← 成功模式
│   └── daily/          ← 每日工作记录
├── skills/             ← Skill 库
└── copilot-meta-rules/ ← 元规则体系（本文件所在目录）
    ├── META-RULES.md          ← 主文件（核心+索引）
    ├── sedimentation-path.md  ← 沉淀之路详细规则
    ├── counter-mechanism.md   ← 计数机制详细规则
    ├── memory-management.md   ← 记忆管理详细规则
    ├── quality-assurance.md   ← 质量保障详细规则
    ├── safety-guardrails.md   ← 安全护栏详细规则
    ├── global-local-rules.md  ← 全局与局部规则
    └── initialization-protocol.md ← 初始化协议（本文件）
```

---

## 4. 初始化确认

初始化完成后，Agent **必须**向用户确认：

```
✅ 工作区记忆体系已初始化。

已创建以下文件和目录：
- 核心文件：AGENTS.md, SOUL.md, MEMORY.md, TODO.md
- 记忆文件：memory/experiences.md, counters.md, facts.md, principles.md
- 记忆目录：memory/decisions/, learnings/, patterns/, daily/
- Skill 目录：skills/
- 元规则体系：copilot-meta-rules/（8个文件）

请根据项目实际情况修改 AGENTS.md 和 SOUL.md 中的占位内容。
```

---

## 5. 初始化的注意事项

- 初始化创建的文件中的占位内容（如 `[项目名称]`）需要用户根据实际情况填写
- Agent 不应自动填写占位内容，除非用户明确提供了相关信息
- `copilot-meta-rules/` 目录应纳入版本控制（Git），确保团队成员共享同一套元规则
- `memory/` 目录下的文件也应纳入版本控制，但 `memory/daily/` 可以选择不纳入（每日工作记录可能较为琐碎）
