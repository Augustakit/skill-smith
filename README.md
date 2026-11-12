# skill-smith · 技能工坊

> **给 AI 技能做体检、做创作、做审核** —— 一个中文、跨平台、只给建议不擅自动手的 Agent Skills 工具链。

## 它解决什么问题

技能（Agent Skill）是"`SKILL.md` + 若干资源文件"的文件夹。写一个能用的不难，写一个**规范、能触发、跨平台不坏、安全**的很难：

- description 写错 → 永远不触发，或到处乱触发
- 结构不规范 → 换个平台就失效
- 装了外部技能 → 不知道它会不会乱读文件、乱发请求
- 越写越长 → 每次触发吃掉几万 token

skill-smith 把这些问题拆成 **5 条流程**，每条都先出报告、再给选项、由你决定。

## 五条流程

| 流程 | 什么时候用 | 产出 |
|---|---|---|
| **A 新建** | 想做技能、想把流程变成技能 | 从提问到草稿到自检的完整路径 |
| **B 改造** | 自己的技能不太灵 | 只读体检报告 + 改造方案选项 |
| **C 审核** | 想装别人的技能 | 来源、供应链、安全、冲突四项报告 |
| **D 排查** | 不触发 / 乱触发 / 报错 / 换平台失效 | 症状对照表 + 追问三件事 |
| **E 复核** | 改完了 | 复跑清单 + 交付检查 |

## 三条底线

1. **只提建议，不替你拍板。** 任何写操作前先说「改什么、为什么、风险」，等你确认。
2. **默认不动。** 删除 / 覆盖 / 移动一律先问，并给备份方案。
3. **一次一问，一问带选项。** 每个问题 2–4 个答案，标推荐项和取舍。

## 它知道什么

| 领域 | 内容 |
|---|---|
| **规范** | frontmatter 字段与上限、目录结构、渐进式披露 |
| **平台差异** | Claude Code / Codex / Kimi / ZCode / DSH / WorkBuddy 的路径、上限、调用方式 |
| **写作方法** | description 公式、自由度分级、常见坑、五种结构模式 |
| **测试** | 最小测试、触发测试、A/B 盲测、量化 |
| **安全** | 供应链检查、危险 vs 正常模式识别（防误杀） |

## 安装

把整个文件夹放进平台技能目录：

| 平台 | 路径 |
|---|---|
| Claude Code | `~/.claude/skills/skill-smith/` |
| Codex | `~/.agents/skills/skill-smith/` |
| Kimi | `~/.agents/skills/skill-smith/` |
| ZCode | `~/.agents/skills/skill-smith/` |
| DSH | `~/.agents/skills/skill-smith/` |

## 怎么用

本技能**默认手动触发**（`disable-model-invocation: true`），不占上下文、不会误触发。

| 平台 | 手动调用 |
|---|---|
| Claude / DSH / ZCode | `/skill-smith` 或 `/skill:skill-smith` |
| Kimi | `/skill:skill-smith` |
| Codex | `$skill-smith` |

想让它自动触发，删掉 frontmatter 里的 `disable-model-invocation: true` 即可。

### 常见说法

- 「帮我做个技能：把周报模板填好」
- 「我这个技能老不触发，帮看看」
- 「这个 GitHub 上的技能能不能装」
- 「它在这个平台上报错了」

## Codex 侧车文件

本技能附带 `agents/openai.yaml`，用于在 Codex 的界面里显示技能名称、简介与默认提示。其他平台不读取该文件，可忽略。

## 目录结构

```
skill-smith/
├── SKILL.md                  # 主流程：A–E 五条路径
├── LICENSE                   # MIT
├── NOTICE.md                 # 参考来源与致谢
├── agents/openai.yaml        # Codex UI 元数据（其他平台不用）
└── references/
    ├── platform-matrix.md    # 各平台路径、上限、调用方式
    ├── checklist.md          # 体检清单（阈值的唯一出处）
    ├── authoring.md          # 写作与测试方法
    ├── report-template.md    # 体检报告模板
    ├── sources.md            # 官方文档与仓库清单（怎么查、怎么更新）
    └── taxonomy.md           # 十一类技能与检查重点
```

## 设计取舍

- **规则前置、细节下沉**：正文留不可跳过的检查项，长清单进 `references/`，按需加载。
- **数字只有一个出处**：所有阈值（1024 字符、500 行、token 预算）只写在 `checklist.md`，其他地方给指针。
- **以 Anthropic 官方规范为准**：与其他平台冲突时按官方走；官方没规定的才看平台差异。
- **防御性提问**：拿不准就问，不猜；每个问题带选项和退路。

## 兼容性

| 平台 | 状态 |
|---|---|
| Claude Code | ✅ |
| Codex | ✅（已附 `agents/openai.yaml`） |
| Kimi | ✅（description 108 字符 ≪ 1024；手动触发方式未实测） |
| ZCode | ✅ |
| DSH | ✅ |
| WorkBuddy | ✅（可读） |

## 许可

MIT，见 [LICENSE](LICENSE)。参考来源与致谢见 [NOTICE.md](NOTICE.md)。

---

**短描述（Github 仓库简介）**：

> 给 AI 技能做体检、创作与审核的中文工具链：五条流程、只提建议、跨平台可用。