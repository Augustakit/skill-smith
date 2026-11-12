# 来源登记表（用于复查与更新）

> **用途**：规范会变。这张表记录每个平台的官方来源与官方仓库，复查时直接抓这些地址，不要凭记忆。

## 一、官方文档与仓库

| 平台 | 官方文档 | 抓取状态 | 官方仓库 |
|---|---|---|---|
| **Anthropic / Claude** | `platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.md`<br>`.../best-practices.md` | ⚠️ 跨域重定向，需人工打开 | `anthropics/skills` ★193,116 |
| **OpenAI Codex** | `developers.openai.com/plugins/build/skills.md` | ❌ 403（网页全封）<br>✅ **GitHub API 可读** | `openai/skills` ★28,693<br>`openai/codex` ★134,918 |
| **Kimi** | `moonshotai.github.io/kimi-cli/zh/customization/skills.html` | ✅ 可抓 | `MoonshotAI/kimi-cli` ★11,417 ⚠️ **已归档** |
| **Gemini CLI** | `geminicli.com/docs/cli/creating-skills` | ✅ 可抓 | `google-gemini/gemini-cli` ★108,782 |
| **Agent Skills 规范** | `modelcontextprotocol.io/extensions/skills/overview.md` | ✅ 可见 | `agentskills/agentskills` ★27,197 |
| **ZCode** | 本地：`~/.zcode/cli/plugins/cache/zcode-plugins-official/*/skills/*/SKILL.md`<br>配置：`~/.zcode/cli/config.json` | ✅ 本地可读 | — |
| **DSH** | 本地：程序目录 `app.asar`<br>技能管理：`@michengai/dsh-skills-manager/lib/core.js` | ✅ 本地可读 | — |
| **技能校验器** | — | — | `agent-ecosystem/skill-validator`（MIT, Go） |

## 二、怎么从 GitHub 取内容（网页被封时）

`api.github.com` 可用，`github.com` / `raw.githubusercontent.com` 常被拦。

| 目标 | 请求 |
|---|---|
| 仓库概况（星标/更新时间） | `https://api.github.com/repos/<owner>/<repo>` |
| 目录列表 | `https://api.github.com/repos/<owner>/<repo>/contents/<path>` |
| 读单个文件 | 同上，返回 `content` 字段是 **base64**，解码即原文 |

**OpenAI 实测路径**：`skills/.system/skill-creator/`（含 SKILL.md 369 行、agents/、references/、scripts/）。

## 三、技能市场（找现成技能）

| 站点 | 接口 |
|---|---|
| **ClawHub** | `https://clawhub.ai/api/v1/search?q=<词>&limit=<n>`（JSON，results[] 含 ownerHandle/slug/displayName/summary/downloads） |
| **ClawHub CN** | `https://clawhubcn.com/skills`、`/plugins`、详情 `/skills/<owner>/<slug>` |
| **GitHub** | `https://api.github.com/search/repositories?q=topic:agent-skills&sort=stars` |

## 四、WorkBuddy 本地来源（无需联网）

| 位置 | 内容 |
|---|---|
| `~/.workbuddy/skills/` | 用户安装的技能（目录名 `<名字>__<来源>`） |
| `~/.workbuddy/plugins/cache/workbuddy-builtin/` | 内置插件（含 `skill-skill-creator` 265 行） |
| `~/.workbuddy/plugins/marketplaces/` | 市场插件（含 codebuddy 官方版 skill-creator 479 行） |
| `~/.workbuddy/.skill-list-cache.json` | 技能清单缓存 |

实测：WorkBuddy 内置 skill-creator 是 Anthropic 版分叉，新增 `agent_created` 与 `userModified` 两个机制。

## 五、抓取失败时的降级顺序

1. 官方文档站
2. 官方 GitHub 仓库（**优先用 API**，见第二节）
3. 官方博客 / release notes
4. 二手总结（**只能当线索，必须回原文核实**）
5. 都没有 → 标「未验证」，不写结论

## 六、怎么更新本技能

1. 抓上面「可抓」的地址，对比 `references/platform-matrix.md` 里的数字
2. 有差异 → 改矩阵（不要写入日期或个人环境信息）
3. GitHub 仓库用 `api.github.com/repos/<owner>/<repo>` 取 `stargazers_count`、`pushed_at`
4. 抓不到（403 / 重定向 / 需登录）→ 保留「未验证」，**不要猜**
5. 更新完跑一遍 `references/checklist.md` 自检
6. 发现新领域 → 同步更新 `references/taxonomy.md` 的类别