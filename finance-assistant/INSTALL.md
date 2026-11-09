# 安装财务助手

这个模板只有一份人设 `SOUL.md`。技能和 MCP 都是 GitHub 上的开源项目，地址列在 `SOUL.md` 的“首次自动安装”里。

## 最简单的装法（所有平台通用）

1. 在你的平台新建一个 Bot / Agent / 机器人，名字填“财务助手”。
2. 把 `SOUL.md` 全文粘进它的人设或系统提示词。
3. 开始第一次对话，随便说一句话。它会列出要装的技能和 MCP，你回复“同意”，它就自己装好并逐项汇报。
4. 按它的提示重开对话或重启应用，让 MCP 生效。

前提是这个 Bot 能用终端或读写文件。不能的话，它会把清单给你，按下面手动装。

## 手动装

**技能**：打开 `SOUL.md` 里每个技能的地址，下载整个文件夹，放进：
- Hermes：`%LOCALAPPDATA%\hermes\profiles\<机器人名>\skills\`
- Claude Code / Cowork：`~/.claude/skills/`，或项目里的 `.claude/skills/`
- Codex：`~/.codex/skills/`
- OpenBot、Grok Bot：在 Agent 的技能页上传每个文件夹里的 `SKILL.md`

**MCP**（可选）：
- Hermes：机器人设置 → MCP → 添加。excel 填命令 `uvx`、参数 `excel-mcp-server stdio`；chart 填命令 `npx`、参数 `-y @antv/mcp-server-chart`。
- Claude Code：`claude mcp add excel -- uvx excel-mcp-server stdio`，`claude mcp add chart -- npx -y @antv/mcp-server-chart`
- Codex：在 `~/.codex/config.toml` 加 `[mcp_servers.excel]`，`command = "uvx"`，`args = ["excel-mcp-server", "stdio"]`；chart 同理。

需要先装 Node.js（提供 npx）和 uv（提供 uvx）。

## 第一次用

装好后它会问三个问题：用哪套准则、用什么财务软件、这次要做什么。回答后把单据或表格发给它即可。
