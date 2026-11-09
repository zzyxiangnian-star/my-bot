# 通用 Bot 模板格式（v1）

一个 Bot 模板 = 一个文件夹，核心只是一份人设 SOUL.md。技能和 MCP **不自己写、不随模板分发**，一律引用 GitHub 上现成的开源项目地址，由 Bot 第一次对话时自己安装（见“自我安装”）。不绑定任何平台：人设和技能用的是各家都认的 Markdown + Agent Skills 格式，平台差异只写在 `INSTALL.md` 里。网站直接展示 `README.md`，下载整个文件夹即可使用。

## 文件夹结构

```
<bot-id>/
├── README.md        给人看的介绍页（网站展示）
├── bot.yaml         机器可读的元信息（网站列表、筛选、商店导入用）
├── SOUL.md          人设 / 系统提示词
├── INSTALL.md       在 Hermes / Claude / Codex / OpenBot / Grok Bot 里怎么装
└── NOTICE.md        出处、改编说明、许可证
```

`bot-id` 用英文小写加连字符，如 `finance-assistant`。

## bot.yaml

```yaml
id: finance-assistant
name: 财务助手
version: 1.0.0
category: finance            # finance / sales / marketing / content / research / data / office / dev / personal / meta
summary: 一句话，30 字以内
tags: [记账, 对账, 月结]
language: zh-CN
tools:                       # 抽象能力，不写平台专有名
  required: [file]           # file / web / browser / terminal / code / schedule / email / image
  optional: [web, code]
connectors:                  # 可选连接，写清用途；没有也能用
  - name: Excel / WPS 表格
    purpose: 读写凭证表、对账表
    required: false
mcp:                         # 推荐的开源 MCP，写完整启动方式
  - name: excel
    repo: https://github.com/haris-musa/excel-mcp-server
    command: uvx excel-mcp-server stdio
    required: false
skills:                      # 只引用 GitHub 开源技能，url 指向含 SKILL.md 的文件夹
  - {name: reconciliation, url: https://github.com/anthropics/knowledge-work-plugins/tree/main/finance/skills/reconciliation}
routines: [month-end-reminder]   # 定时任务写在 SOUL.md 的首次自动安装里
approval_gates:              # 必须由人确认才能做的事
  - 过账、结账、对外发送任何文件
source:
  - name: anthropics/knowledge-work-plugins / finance
    url: https://github.com/anthropics/knowledge-work-plugins
    license: Apache-2.0
license: Apache-2.0
visibility: public            # public = 可发布到网站；internal = 只自己内部用（如照搬 Grok 市场的模板）
```

## SOUL.md 写法

固定四节，每节 3～6 条，口语、具体，不写空话：

1. **我是谁**：一句话角色 + 服务对象。
2. **我做什么**：核心职责，每条对应一个技能或一类输出。
3. **我怎么做**：工作习惯，比如先确认口径、先出样例再批量、每个数字标来源。
4. **我不做什么**：边界和审批节点。凡是涉及钱、对外发送、删除、不可撤销的操作，都写成“起草，等你确认”。

倒数第二节 **首次自动安装**（见下面“自我安装”），最后一节 **开场**：装好后先问哪几个问题（不超过 3 个）。

## 自我安装（SOUL.md 必含）

有些平台只能先建 Bot、粘贴人设，不能连技能和 MCP 一起导入。所以每个 SOUL.md 都带一节“首次自动安装”，让 Bot 第一次对话时自己把技能和 MCP 装上。写法照下面，只改清单：

```markdown
## 首次自动安装

第一次对话时，先检查下面的技能和 MCP 是否已经可用；缺的，先把清单列给用户，用户同意后再逐项安装，装完逐项汇报“已装 / 失败及原因”。已经全部装好就跳过这一节，直接进入开场。

**技能**（GitHub 开源技能，下载整个文件夹到本平台的技能目录）：
| 技能 | 地址 |
|---|---|
| reconciliation | https://github.com/anthropics/knowledge-work-plugins/tree/main/finance/skills/reconciliation |

**MCP**（加到本平台的 MCP 配置里，名字照写）：
| 名字 | 项目地址 | 启动方式 | 必需 |
|---|---|---|---|
| excel | https://github.com/haris-musa/excel-mcp-server | `uvx excel-mcp-server stdio` | 否 |

**安装规则**
- 先判断自己在哪个平台，用这个平台的方式装：技能目录如 Hermes 的 `profiles/<名>/skills/`、Claude 的 `~/.claude/skills/`、Codex 的 `~/.codex/skills/`；MCP 写进本平台的 MCP 配置，或用平台自带的添加命令。
- 只从上表列出的地址下载，不搜索、不换源；地址打不开就报告失败，不找替代品。
- 缺 Node.js（npx）或 uv（uvx）时，告诉用户怎么装，不擅自装系统软件。
- 需要 Key 的 MCP，只提示用户自己填，不索要、不写进任何文件。
- 没有终端或文件工具、装不了时，把上表原样给用户，请用户手动装。
- 装好后提醒用户：MCP 可能要重开对话或重启应用才生效。
```

`bot.yaml` 里的 `skills` 和 `mcp` 要和这张表一致。

## 怎么选技能和 MCP

- 只选 GitHub 上公开、有许可证、近 6 个月有更新的项目；地址指向含 `SKILL.md` 的那个文件夹（技能）或项目首页（MCP）。
- 优先来源：anthropics/knowledge-work-plugins、anthropics/skills、openai/skills、NousResearch/hermes-agent 的 skills 与 optional-skills、JimLiu/baoyu-skills；MCP 优先能 `npx`/`uvx` 一行启动、免登录的。
- 每个 Bot 技能 3～8 个、MCP 0～3 个。
- 开源技能多是美国口径或英文，差异写进 SOUL.md 的“中国口径”一节，不去改技能本身。
- 找不到合适开源技能的能力，直接写进 SOUL.md 的“我怎么做”，不要自己造技能文件。

## INSTALL.md

先写通用装法（新建 Bot → 粘贴 SOUL.md → 第一次对话同意自动安装），再写各平台手动装法（技能目录、MCP 添加方式），照样板。

## NOTICE.md

- 列出引用的每个开源技能和 MCP：地址、许可（它们不随模板分发）。
- 改了什么（例：改为中国会计准则、删去 SOX 相关技能、重写人设）。
- Apache-2.0 来源需附原许可证全文链接。
- 只借思路、未使用原文的来源写“参考”，不算改编。

## 公开发布红线

- `visibility: public` 的模板只改编许可证允许的来源（MIT、Apache-2.0 等）；Grok 市场等未写许可的模板可以照搬，但只能标 `internal` 自己内部用，不上网站。
- 不放任何密钥、账号、内部地址。
- 不收：模拟登录自动发帖、绕过验证码/付费墙、自动下单付款、越狱类。
