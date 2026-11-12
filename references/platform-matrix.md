# 平台兼容矩阵

## 目录与限制

| 平台 | 用户级目录 | 项目级目录 | description 上限 | 认 `disable-model-invocation` |
|---|---|---|---|---|
| **OpenAI Codex** | `~/.agents/skills` | `<项目>/.agents/skills`（从 cwd 向上扫到仓库根） | 未明确 | ⚠️ 不认该字段；要用 `agents/openai.yaml` 的 `policy.allow_implicit_invocation: false` |
| **Kimi Code CLI**（⚠️ 旧 `kimi-cli` 仓库已归档，以当前文档为准） | 品牌组：`~/.kimi/skills`、`~/.claude/skills`、`~/.codex/skills`；通用组：`~/.config/agents/skills`（推荐）、`~/.agents/skills` | `.kimi/skills`、`.claude/skills`、`.codex/skills`、`.agents/skills` | **1–1024 字符**（规范值） | 未验证 |
| **DSH** | `~/.agents/skills`、`~/.dsh/skills` | `<项目>/.dsh/skills`、`<项目>/.agents/skills` | 无硬限制（占上下文） | ✅ 已验证 |
| **ZCode** | `~/.agents/skills` | （未验证） | **1024，超了报错** | ✅ 已验证 |
| **Claude Code** | `~/.claude/skills` | `<项目>/.claude/skills` | 路由预算 1536，实际建议 ≤1024 | ✅ 原生 |
| **Gemini CLI / Antigravity** | `~/.gemini/skills` | `<项目>/.gemini/skills` | 未明确 | 未验证 |
| **WorkBuddy** | `~/.workbuddy/skills/<名字>__<来源>/` | `.workbuddy/skills` | 未知 | 认 `disable-model-invocation`（有迁移文件佐证） |
| **OpenCode** | `~/.config/opencode/skills` | `<项目>/.opencode/skills` | 未知 | 未知 |

> **来源等级**：OpenAI 官方网页 403，但 **GitHub 仓库已核实**（`openai/skills` 的 skill-creator SKILL.md，369 行）。仅「CLI 发现规则 / `$skill` 调用」来自第三方对比文章，属二手。
> Codex 还支持系统级 `/etc/codex/skills`（管理员部署）。

## 已确认的通用事实

1. **`.agents/skills` 是跨工具标准位** —— Kimi 列为「通用组」，**Codex 原生使用**，DSH 与 ZCode 实测会用。
2. **description 规范上限 = 1024 字符** —— Kimi 文档明写「1-1024 字符」，与 ZCode 的报错阈值一致。
3. **可移植最小集** —— `name` + `description` + Markdown 正文 + 参考文件；平台差异全部隔离在外层。
4. **目录三件套** —— `scripts/`、`references/`、`assets/`，Kimi / Gemini / Anthropic 口径一致。
5. **SKILL.md ≤500 行** —— 各家一致，细节移进参考目录。
6. **常驻指令文件不同** —— Claude 用 `CLAUDE.md`，Codex 用 `AGENTS.md`，别混。
7. **官方 frontmatter 字段** —— `name`（≤64，小写/数字/连字符，不以连字符开头结尾）、`description`（≤1024）必填；`license`、`compatibility`（≤500）、`metadata`、`allowed-tools`（Experimental）可选。
8. **目录允许任意额外文件** —— 官方 spec 写 `└── ... Any additional files or directories`；三件套只是「建议」，不是硬性。

## 各平台的发现优先级（同名时谁赢）

| 平台 | 顺序（高 → 低） |
|---|---|
| **Kimi** | Project > User > Extra > Built-in；品牌组优先于通用组 |
| **ZCode** | `<项目>/.zcode/skills` > `<项目>/.agents/skills` > `~/.zcode/skills` > `~/.agents/skills` |
| **DSH** | 项目 `.dsh/skills` > 项目 `.agents/skills` > `~/.dsh/skills` > `~/.agents/skills` |
| **Codex** | `$CWD/.agents/skills` > `$REPO_ROOT/.agents/skills` > `$HOME/.agents/skills` > `/etc/codex/skills` |

**含义**：`.zcode/skills`、`.kimi/skills` 这类品牌目录是「覆盖位」；`.agents/skills` 是「共享位」。要临时压过某个技能，才写到品牌目录。

## 各平台的额外能力 / 差异

| 平台 | 特点 |
|---|---|
| **Codex** | 调用方式是 `$skill` 提及（输入 `$` 选择）或 `/skills`；手动触发靠 `agents/openai.yaml` 的 `policy.allow_implicit_invocation: false`；UI 元数据也放该 sidecar；`AGENTS.md` 是常驻指令 |
| **Claude Code** | 调用是 `/skill-name` 斜杠命令；额外 frontmatter：`allowed-tools`、`argument-hint`、`context: fork`、`agent`、`paths`、hooks；`CLAUDE.md` 是常驻指令 |
| **Kimi** | 除 `<name>/SKILL.md` 外，**单个扁平 `.md` 也算一个技能**（name = 文件名）；同名时子目录优先并记警告。description 回退链：frontmatter → 正文首个非空行（超 240 字截断）→「No description provided.」；`compatibility` 字段上限 500 字符；加载用 `/skill:<name>` |
| **Gemini CLI** | 技能落在 `.gemini/skills/`；自带 `skill-creator`；标准三件套 |
| **ZCode** | 技能页会给报错列表；内置插件里已有 `docx`/`pdf`/`pptx`/`xlsx`/`skill-creator`，与用户技能**同名** |
| **DSH** | `disable-model-invocation: true` 的技能**不向模型展示**，只在界面可调用；支持技能开关 |

## WorkBuddy 专属规则

| 规则 | 说明 |
|---|---|
| **必须写 `agent_created: true`** | 平台自带的 `skill_manage` 靠这个标记才能后续修改/删除技能 |
| **市场安装的技能带 meta** | 目录里有 `_skillhub_meta.json`（SkillHub 源）或 `_knot_meta.json`（Knot 源） |
| **改完必须标 `userModified: true`** | 否则市场更新会**静默覆盖**用户修改 |
| **目录名格式** | `<技能名>__<来源>`，例如 `skill-creator-1__skillhub` |
| **内置插件路径** | `plugins/cache/workbuddy-builtin/<插件>/<版本>/...` |
| **它分发的是 Anthropic 原版** | 实测 SkillHub 上的 `skill-creator-1` 就是 Anthropic 那份（485 行、18 文件，逐字节一致） |

## 跨工具互相污染（重要）

DSH 的技能管理器会扫描 20+ 个工具的技能目录，包括 `~/.workbuddy/skills`、`~/.claude/skills`、`~/.codex/skills`、`~/.cursor/skills` 等。

**后果**：在 A 工具装的技能，会自动出现在 DSH 里；在 DSH 删掉的技能，只要别的工具目录里还有，**就会重新出现**。

**处理**：
- 确认来源：同名技能要查它到底是哪个目录来的
- 不想让 DSH 看到：在该工具的目录里处理，或在 DSH 里停用该来源
- 各工具目录标记 `mutable: false`（只读）但 `toggleable: true`（可开关）

## 命名冲突处理

| 情况 | 后果 | 做法 |
|---|---|---|
| 同平台重名 | 高优先级目录赢，低优先级被覆盖 | 明确谁该赢，或改名 |
| 与内置插件同名 | 哪份生效不确定 | 改名，或停用内置 |
| 跨平台同名 | 各平台各读各的，互不影响 | 无需处理 |

## 写「一份技能多平台用」的正确姿势

1. **核心保持最简**：`name` + `description` + Markdown 正文 + 参考文件
2. **平台差异隔离在平台层**：手动触发在 Claude 写 `disable-model-invocation`，在 Codex 写 `agents/openai.yaml`
3. **正文不要出现平台专属工具名**（`present_files`、`/skill-test` 等）
4. **同一个技能要在两处生效**：把内容放 `~/.agents/skills`，Claude 需要时再放一份到 `~/.claude/skills`（或做目录链接）