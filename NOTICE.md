# 致谢与参考来源

本技能是原创编写的，没有复制任何项目的源代码或大段原文。以下来源提供了**事实、规范与设计思路**，按诚实原则列出。

## 一、规范与文档

| 来源 | 许可 | 用到了什么 |
|---|---|---|
| Agent Skills 规范 · https://agentskills.io/specification | Apache-2.0 | frontmatter 字段定义与长度上限、目录结构 |
| Agent Skills 最佳实践 · https://agentskills.io/skill-creation/best-practices | Apache-2.0 | 渐进披露、按脆弱度调整自由度、常见坑等模式 |
| Anthropic 官方技能库 · https://github.com/anthropics/skills | 见仓库 | 官方写法对标、优秀技能样例 |
| OpenAI Codex 技能生成器 · https://github.com/openai/skills | 见 skills/.system/skill-creator/LICENSE.txt | 上下文是公共资源、自由度分级、六步流程 |
| Kimi 自定义技能文档 · https://moonshotai.github.io/kimi-cli/zh/customization/skills.html | 见站点 | 目录优先级、1024 字符限制、扁平 .md 支持 |
| Gemini CLI 技能文档 · https://geminicli.com/docs/cli/creating-skills | 见站点 | 目录位置、三件套 |

## 二、工具与问卷

| 来源 | 许可 | 用到了什么 |
|---|---|---|
| skill-validator · https://github.com/agent-ecosystem/skill-validator | MIT | 检查项与阈值思路 |
| ClawHub / ClawHub CN | 站点 | 技能市场检索接口 |

## 三、方法论文献

- SkillsBench（2026-02）：小而模块化的技能优于大型内容堆。
- 一次从 47 个测试技能合并回 12 个的公开重构经验：按能力拆、不按业务拆。
- 87 个技能 898 KB → 27 KB 的两层结构案例（-97%）。

## 四、说明

- 本技能中的规则、阈值、流程均为**自行组织与改写**，非原文照搬。
- 若发现任何来源需要更正署名，请提交 issue。
- 本技能以 MIT 许可发布；第三方来源的权利归各自作者所有。
