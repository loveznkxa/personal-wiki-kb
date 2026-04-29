---
name: personal-wiki-kb
description: >
  Activate when user mentions "wiki", "knowledge base", "ingest", "ingest_source", 
  "query_knowledge", "persist_knowledge", "audit_wiki", "raw/", or explicitly 
  wants to build / manage a personal wiki.
  Provides four modules: ingest (compile raw into wiki), query (answer from wiki), 
  persist (write new conclusions into wiki), and audit (health check). 
  Designed for long-term knowledge compounding with strict source immutability 
  and confidence tracking.
---

# 个人 Wiki 知识库 (Personal Wiki KB)

## 概述
将知识管理从一次性的"信息存储"升级为持续的"知识工程"。系统像编译器一样工作：**Raw Sources (源文件) → Compiled Wiki (结构化知识网络)**。

### 核心概念
- **Raw Sources** (`raw/` 目录): 原始输入材料，**不可修改 (Immutable)**，是信息的"真相之源"
- **The Wiki** (`wiki/` 目录): LLM 主动生成的 Markdown 文件集合，包括实体页、主题总结页、对比分析页
- **The Schema**: 定义页面格式、更新逻辑、冲突处理、版本规则的元规则集
- **Schema 扩展点**: 用户可通过 `schema-extensions/` 目录自定义新知识类型

### 目录约定
> 所有路径均相对于项目根目录（即包含 `raw/` 和 `wiki/` 的目录）。

project_root/
├── raw/                  # 原始数据（用户上传源文件，只读）
├── wiki/                 # 编译后的结构化知识库
│   ├── index.md          # 目录索引（所有知识的入口）
│   ├── log.md            # 操作日志（记录每次写入/修改/审计）
│   ├── entities/         # 实体页面（人、公司、概念等）
│   ├── topics/           # 主题总结页面
│   ├── comparisons/      # 对比/分析页面
│   └── archive/          # 已归档的旧知识（知识衰减机制）
└── .agent/skills/personal-wiki-kb/
    ├── SKILL.md          # 本文件
    ├── references/       # 工作流详细说明及辅助文档
    └── schema-extensions/ # 用户自定义知识类型模式

---

## 工作流一览
本 Skill 提供四个独立模块。**执行任何模块前，必须先读取对应的详细规则文件**。

| 模块 | 触发场景 | 详细规则文件 |
|------|----------|----------------|
| **摄入** (`ingest_source`) | 用户上传原始数据 (`.md` 或 `.json`) 到 `raw/` | `references/ingest_source.md` |
| **查询** (`query_knowledge`) | 用户提出自然语言问题，需要基于 Wiki 回答 | `references/query_knowledge.md` |
| **回写** (`persist_knowledge`) | 查询、审计等过程产生新结论、矛盾或深度阐释，需写入 Wiki | `references/persist_knowledge.md` |
| **维护** (`audit_wiki`) | 用户要求"检查知识库"或系统触发定期审计 | `references/audit_wiki.md` |

### 辅助文档（按需加载）
- 快速启动指南: `references/quickstart.md`
- 故障排除: `references/troubleshooting.md`

---

### 全局约束（所有模块必须遵守）
- `raw/` 目录中的源文件**只读，不可修改**
- 任何写入操作（摄入、回写）必须在呈现分析报告或草案后，**等待用户明确回复 `Y` 或 `N`**，确认后才执行
- 所有 Wiki 页面必须包含 YAML frontmatter 元数据（格式见各详细文件）
- **操作日志**: 每次成功写入、更新或审计修复后，必须在 `wiki/log.md` 末尾追加记录：
  `YYYY-MM-DD HH:MM [模块名] 操作描述`
- **人工复核标记**: 置信度为"低"的页面顶部必须加 `> [⚠️ 待复核 - 置信度:低]`；涉及具体数据、统计结论或单来源推断的段落前标注 `[需人工验证]`
- **回写触发**: 查询或审计结束后若产生跨文档新结论、发现的矛盾、深度新阐释，必须自动调用 `persist_knowledge` 模块生成草案并等待用户确认
- **辅助文档**: 遇到操作异常或用户指出问题时，主动查阅 `references/troubleshooting.md`
- **禁用自动递归**: 单次用户请求中，每个模块只允许被调用一次。回写完成后不得自动再次触发查询或审计，除非用户明确要求。

---

## 索引格式 (`wiki/index.md`)
索引文件必须严格按以下四类维护，每项包含链接、简短描述和置信度：
- **实体** (`## 实体 (Entities)`)
- **主题** (`## 主题 (Topics)`)
- **对比分析** (`## 对比分析 (Comparisons)`)
- **待创建页面** (`## ⏳ 待创建页面`)

> 索引仅用于快速定位，详细内容以各 Wiki 页面为准。

---

## Schema 扩展点（自定义知识类型）
如需添加 Prolog 规则、数学公式、代码片段等自定义知识类型：
1. 在 `schema-extensions/` 下创建 `custom-type.md`
2. 定义格式：名称、字段结构、索引规则
3. 在 `index.md` 中添加自定义类型章节
4. 摄入时 Agent 将自动识别并应用自定义模式

---

## 维护节奏建议
- **定期审计**: 建议每摄入 10 个源文件，或每 30 天，手动运行一次 `audit_wiki`
- **自动提醒**: 可在 `wiki/index.md` 上方添加 `下次建议审计日期`，Agent 查询时可据此提醒
- 审计报告末尾将自动给出下一次建议审计的时间，并写入 `log.md`