# Knowledge Blindspot Journal

`knowledge-blindspot-journal` 是一个基于 Agent Skills 开放标准的可复用 skill，用于把一次真实的编码、排障、联调或需求分析线程，整理成一份基于证据的知识盲点沉淀文档。

它主要解决两件事：

- 识别你在当前线程里真正暴露出来的知识盲点
- 针对这些盲点给出解释、正确心智模型和后续学习资料

这个 skill 的目标不是绑定某一个 AI 工具，而是尽可能兼容支持 Agent Skills 的工具生态，例如：

- Codex
- Claude Code
- OpenCode
- 其他兼容 Agent Skills 标准的代理工具

## 这个 Skill 会产出什么

该 skill 生成的 Markdown 文档通常包含：

- 线程概览
- 已确认盲点
- 高概率盲点
- `review` 模式下的待确认候选
- 不纳入归档的项及原因
- 对已确认盲点的完整讲解
- 对高概率盲点的简要讲解
- 官方优先的学习资料推荐
- 后续学习行动建议

## 核心能力

- 保守识别盲点  
  不是所有“向 AI 求助”的行为都算知识盲点。

- 置信度分层  
  使用 `confirmed`、`likely`、`tentative`、`exclude` 四类结果。

- 两种输出模式  
  - `review`：先筛选候选，再决定哪些应该归档  
  - `strict`：低噪声地直接输出最终归档结果

- 偏教学型输出  
  - `confirmed` 盲点会附带完整讲解  
  - `likely` 盲点会附带简要讲解  
  - `tentative` 和 `not archived` 默认不做讲解

## 兼容性

这个 skill 采用标准的 `SKILL.md + 可选资源目录` 结构，便于在多种代理工具之间复用。

目前推荐的兼容目标：

- Codex：可作为本地 skill 安装，也可以从 GitHub 仓库安装
- Claude Code：支持 `.claude/skills/<name>/SKILL.md`
- OpenCode：支持 `.opencode/skills/<name>/SKILL.md`，同时兼容 `.claude/skills/` 和 `.agents/skills/`

如果某个工具兼容 Agent Skills 开放标准，一般只需要把 `knowledge-blindspot-journal/` 目录放到该工具可发现的 skills 目录中即可。

## 输出路径

这个 skill 采用工具中立的默认输出目录：

`./.ai/knowledge-blindspots/`

路径覆盖优先级如下：

1. 当前请求里显式指定的路径
2. 环境变量 `AI_KNOWLEDGE_BLINDSPOTS_DIR`
3. 项目配置 `./.ai/knowledge-blindspot-journal.json`
4. 用户全局配置 `~/.ai/knowledge-blindspot-journal.json`
5. 默认路径 `./.ai/knowledge-blindspots/`

配置示例：

```json
{
  "output_dir": "D:/ai-notes/knowledge-blindspots",
  "filename_pattern": "{date}_{topic}.md",
  "default_mode": "review"
}
```

## 仓库结构

```text
knowledge-blindspot-journal-repo/
  README.md
  knowledge-blindspot-journal/
    SKILL.md
    agents/
      openai.yaml
    references/
      explanation-and-resources.md
      output-path-strategy.md
      output-template.md
      zh-mode-trigger-strategy.md
```

说明：

- 仓库根目录用于 GitHub 分发说明
- 真正的 skill 内容位于 `knowledge-blindspot-journal/`
- 安装到其他工具时，通常复制的是 `knowledge-blindspot-journal/` 这个目录

## 从 GitHub 安装 / 引入

这个仓库本身适合作为 GitHub 分发源。

不同工具的安装方式可能不同，但总体思路一致：

1. 从 GitHub 获取仓库内容
2. 找到 `knowledge-blindspot-journal/` 目录
3. 放到目标工具支持的 skills 目录中

### Codex

如果使用 Codex，可以通过 `skill-installer` 从 GitHub 安装：

```text
Use $skill-installer to install the skill from GitHub repo <owner>/<repo> path knowledge-blindspot-journal.
```

也可以手动复制到：

- Windows: `%USERPROFILE%\\.codex\\skills\\knowledge-blindspot-journal`
- macOS/Linux: `~/.codex/skills/knowledge-blindspot-journal`

### Claude Code

Claude Code 支持将 skill 放在：

- 项目级：`.claude/skills/knowledge-blindspot-journal/`
- 用户级：`~/.claude/skills/knowledge-blindspot-journal/`

因此你可以：

1. 从 GitHub clone 或下载本仓库
2. 将 `knowledge-blindspot-journal/` 目录复制到上述任一目录

### OpenCode

OpenCode 支持将 skill 放在：

- 项目级：`.opencode/skills/knowledge-blindspot-journal/`
- 用户级：`~/.config/opencode/skills/knowledge-blindspot-journal/`

OpenCode 还兼容以下目录：

- `.claude/skills/knowledge-blindspot-journal/`
- `~/.claude/skills/knowledge-blindspot-journal/`
- `.agents/skills/knowledge-blindspot-journal/`
- `~/.agents/skills/knowledge-blindspot-journal/`

因此在 OpenCode 中，也可以直接从 GitHub 获取仓库后复制 `knowledge-blindspot-journal/` 目录进行安装。

## 使用示例

`review` 模式：

```text
Use $knowledge-blindspot-journal to review this thread first, separate real blindspots from noise, and then draft the archive.
```

`strict` 模式：

```text
Use $knowledge-blindspot-journal in strict mode to write the final markdown archive for this thread.
```

常见中文意图可以理解为：

- 先帮我判断哪些算真正的知识盲点，再生成 md
- 只归档这次我真正不会的点
- 我怕误判，先列候选
- 只保留确定的盲点，输出最终版

## 适用场景

这个 skill 更适合在这些场景后使用：

- 处理真实 bug 之后
- 排查环境、联调问题之后
- 一次功能设计讨论结束后，但你感觉有认知短板暴露出来
- 某个线程里你多次追问“为什么这样能工作”

## 说明

- 当前仓库已经按可分发的 skill 目录结构组织，可以直接上传到 GitHub
- 输出路径和沉淀文档本身是工具中立的，不绑定 `.codex/`、`.claude/` 或 `.opencode/`
- 如果运行时允许联网，学习资料推荐应优先使用当前官方文档和一手资料
- 不同工具的“自动安装”能力不同，但都可以通过 GitHub 分发 + 复制 skill 目录的方式完成接入
