---
title: 'OpenClaw 完全指南：从零搭建你的私人 AI 助理帝国'
date: 2026-05-03
permalink: /posts/2026/05/03/openclaw-complete-guide/
tags:
  - OpenClaw
  - AI Agent
  - 多Agent
  - 飞书
  - 自部署
---

# OpenClaw 完全指南：从零搭建你的私人 AI 助理帝国

## 一、OpenClaw 是什么？

### 1.1 一句话定义

OpenClaw 是一个**自托管的 AI Agent 网关**——它把你常用的聊天软件（飞书、微信、Telegram、Discord、WhatsApp 等）和 AI 大模型（GPT、Claude、Gemini 等）连接起来，让你随时随地通过聊天消息与 AI 助理对话。

### 1.2 它不是什么

- **不是一个聊天应用**：它不替代飞书/微信，而是"寄生"在这些平台上
- **不是一个 AI 模型**：它不训练模型，而是调用你选择的模型 API
- **不是一个简单的转发器**：它有完整的 Agent 运行时、工具调用、会话管理、多 Agent 路由

### 1.3 核心架构

```
你的聊天 App（飞书/微信/Telegram...）
        ↓ 消息
   OpenClaw Gateway（网关）
        ↓ 路由
   AI Agent（Pi/Codex 等）
        ↓ 工具调用
   文件/浏览器/命令行/API...
```

**Gateway（网关）** 是整个系统的大脑：
- 接收来自各渠道的消息
- 路由到对应的 Agent
- 管理会话状态和历史
- 调度工具执行
- 返回结果到聊天窗口

### 1.4 关键特性一览

| 特性 | 说明 |
|------|------|
| **多渠道** | 飞书、微信、Telegram、Discord、WhatsApp、Signal、iMessage 等 20+ 平台 |
| **多 Agent** | 一个网关运行多个独立 Agent，各有自己的人设、工作空间、会话 |
| **工具调用** | 文件读写、Shell 命令、浏览器自动化、网页搜索、代码执行 |
| **Skill 系统** | 可扩展的技能包（邮件、日历、GitHub、天气等） |
| **定时任务** | 内置 Cron 调度器，支持定时提醒、定期巡检 |
| **媒体支持** | 图片、音频、视频、文档的收发和生成 |
| **自托管** | 数据在你自己的机器上，完全可控 |

---

## 二、为什么要用 OpenClaw？

### 2.1 解决的核心痛点

**场景 1：多平台碎片化**

你可能同时用飞书办公、微信社交、Telegram 看频道。每个平台都有自己的 AI 机器人，但它们互相隔离，上下文不共享。OpenClaw 把它们统一到一个 Agent 身份下。

**场景 2：AI 缺乏"手脚"**

ChatGPT 能聊天，但不能帮你发邮件、操作文件、跑脚本。OpenClaw 的工具系统让 AI 能真正"做事"。

**场景 3：数据隐私**

你的对话、文件、API Key 都在你自己的机器上经过，不经过任何第三方服务器。

**场景 4：个性化需求**

你想要的不只是一个通用助手，而是一个了解你习惯、有特定技能、能和你团队协作的专属 Agent。

### 2.2 对比其他方案

| 维度 | ChatGPT/Claude 原生 | Coze/Dify | OpenClaw |
|------|---------------------|-----------|----------|
| 部署方式 | 云服务 | 云服务 | 自托管 |
| 聊天渠道 | 仅官方 App | 有限集成 | 20+ 平台 |
| 工具能力 | 有限 | 可配置 | 完整（文件/Shell/浏览器） |
| 多 Agent | 不支持 | 支持 | 完整隔离 |
| 数据控制 | 在服务商 | 在服务商 | 完全在你 |
| 费用 | 订阅制 | 按量计费 | 仅 API 费用 |

### 2.3 适合谁？

- **开发者**：想要一个能执行代码、操作 Git、管理项目的 AI 助理
- **团队管理者**：需要多 Agent 协作，分工处理不同事务
- **效率达人**：希望 AI 能自动处理邮件、日历、待办
- **隐私敏感用户**：不想把数据交给云平台

---

## 三、怎么做？—— 从零开始部署

### 3.1 环境准备

**系统要求：**
- **操作系统**：Windows 10/11、macOS、Linux 均可
- **Node.js**：推荐 Node 24（Node 22.14+ 也可以）
- **API Key**：至少一个模型提供商的 Key（Anthropic/OpenAI/Google 等）

**安装 Node.js：**

```bash
# macOS (Homebrew)
brew install node

# Linux (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt-get install -y nodejs

# Windows: 下载安装包 https://nodejs.org
```

验证安装：

```bash
node --version  # 应显示 v24.x.x
npm --version   # 应显示 10.x.x
```

### 3.2 安装 OpenClaw

**macOS / Linux：**

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows（PowerShell 管理员）：**

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

**或者用 npm 全局安装：**

```bash
npm install -g openclaw@latest
```

### 3.3 初始化配置

运行交互式引导程序：

```bash
openclaw onboard --install-daemon
```

这个向导会帮你：
1. 选择模型提供商（推荐 Anthropic Claude 或 OpenAI）
2. 填入 API Key
3. 配置基本参数
4. 安装为系统服务

完成后验证：

```bash
openclaw gateway status    # 查看网关状态
openclaw dashboard         # 打开浏览器控制台
```

浏览器访问 `http://127.0.0.1:18789`，看到控制台界面说明一切正常。

### 3.4 配置文件详解

配置文件位于 `~/.openclaw/openclaw.json`（JSON5 格式，支持注释）：

```json5
{
  // Agent 配置
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",  // 工作空间路径
      model: "claude-sonnet-4-20250514",    // 默认模型
    },
  },

  // 渠道配置
  channels: {
    feishu: {
      // 飞书机器人配置
      groupPolicy: "allowlist",
      requireMention: true,
    },
    telegram: {
      // Telegram Bot 配置
      allowFrom: ["your_telegram_id"],
    },
  },

  // 工具配置
  tools: {
    exec: {
      security: "full",  // exec 工具安全模式
    },
  },
}
```

**常用配置命令：**

```bash
# 查看配置
openclaw config get agents.defaults.model

# 修改配置（修改后网关自动重启）
openclaw config set agents.defaults.model "gpt-4o"

# 交互式修改
openclaw configure
```

> ⚠️ **重要提醒**：修改 `openclaw.json` 会触发网关自动重启。在生产环境中，建议在低峰期操作。

### 3.5 连接聊天渠道

#### 3.5.1 飞书（推荐，功能最全）

```bash
openclaw channels login --channel feishu
```

扫描二维码完成授权。飞书支持：
- 私聊（DM）
- 群聊（@机器人触发）
- 话题回复
- 富文本消息
- 文件收发

**群聊配置示例：**

```json5
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["oc_your_group_id"],
      groups: {
        "oc_your_group_id": {
          requireMention: true,  // 需要 @机器人 才响应
        },
      },
    },
  },
}
```

#### 3.5.2 Telegram（最快上手）

```bash
openclaw channels login --channel telegram
```

需要一个 Bot Token（从 @BotFather 获取）。

#### 3.5.3 Discord

```bash
openclaw channels login --channel discord
```

需要创建 Discord Bot 并开启 Message Content Intent。

#### 3.5.4 WhatsApp

```bash
openclaw channels login --channel whatsapp
```

需要一个专用手机号。

#### 3.5.5 其他渠道

OpenClaw 支持 20+ 渠道，完整列表见[官方文档](https://docs.openclaw.ai/channels)。

---

## 四、工作空间与人设定制

### 4.1 工作空间结构

```
~/.openclaw/workspace/
├── AGENTS.md      # Agent 行为规则
├── SOUL.md        # Agent 人设和性格
├── USER.md        # 用户信息
├── TOOLS.md       # 工具使用备忘
├── MEMORY.md      # 长期记忆
├── HEARTBEAT.md   # 心跳任务配置
├── memory/        # 每日记忆文件
│   └── 2026-05-03.md
└── skills/        # 自定义技能
```

### 4.2 定制你的 Agent

**SOUL.md** 定义 Agent 的性格：

```markdown
# 我的 AI 助理

## 核心性格
- 直接、高效，不废话
- 有观点，不做应声虫
- 先动手查资料，再回来汇报

## 语言风格
- 默认中文回复
- 技术讨论可以用英文
- 代码注释用英文
```

**USER.md** 记录你的信息：

```markdown
# 关于我

- 名字：张三
- 时区：Asia/Shanghai
- 偏好：简洁直接
- 工作：后端开发工程师
```

**AGENTS.md** 定义行为规则：

```markdown
# 行为规则

## 安全红线
- 不删除非工作空间文件
- 外发操作必须先确认
- 敏感信息不外泄

## 工作习惯
- 代码改动先跑测试
- 邮件发送前让我过目
- 定时检查邮件和日历
```

---

## 五、多 Agent 通信——组建你的 AI 团队

### 5.1 为什么需要多 Agent？

单个 Agent 万能但不专精。多 Agent 让你：
- **角色分工**：一个管代码，一个管文档，一个管日程
- **权限隔离**：不同 Agent 有不同的工具访问权限
- **并行处理**：多个任务同时进行
- **团队共享**：多人共享一个网关，各自有独立 Agent

### 5.2 创建多 Agent

```bash
# 创建新 Agent
openclaw agents add qianguang

# 查看所有 Agent
openclaw agents list --bindings
```

每个 Agent 有独立的：
- 工作空间（`~/.openclaw/workspace-qianguang/`）
- 会话存储（`~/.openclaw/agents/qianguang/sessions/`）
- 人设文件（SOUL.md、AGENTS.md）
- 技能配置

### 5.3 配置示例：龙豪 + 钳光 协作模式

```json5
{
  agents: {
    list: [
      {
        id: "longhao",
        workspace: "~/.openclaw/workspace",
        // 龙豪：秘书角色，负责协调和安全评估
      },
      {
        id: "qianguang",
        workspace: "~/.openclaw/workspace-qianguang",
        // 钳光：工程师角色，负责执行具体任务
      },
    ],
  },

  // 跨 Agent 通信配置
  tools: {
    sessions: {
      visibility: "all",  // 允许看到其他 Agent 的 session
    },
    agentToAgent: {
      enabled: true,  // 允许 Agent 之间互发消息
    },
  },
}
```

### 5.4 跨 Agent 通信方式

#### 方式一：sessions_send（内部通信）

```python
# 龙豪向钳光发消息
sessions_send(
    sessionKey="agent:qianguang:feishu:direct:ou_xxx",
    message="请处理一下这个任务：..."
)
```

#### 方式二：飞书群聊协作（可见协作）

在飞书群聊中，两个 Agent 通过 @mention 互相沟通：

```
龙豪: @钳光 请检查一下服务器状态
钳光: @龙豪 服务器正常，CPU 15%，内存 42%
```

### 5.5 多 Agent 架构模式

**模式一：主从模式**

```
用户 → 主 Agent（龙豪）→ 分发任务 → 从 Agent（钳光）→ 执行 → 汇报
```

适合：个人使用，一个"管家"统筹多个"执行者"

**模式二：专业分工模式**

```
用户 → 路由层 → 代码 Agent（处理开发任务）
                → 文档 Agent（处理写作任务）
                → 日程 Agent（处理日历邮件）
```

适合：团队使用，按专业领域分配

**模式三：审核模式**

```
用户 → 执行 Agent → 提交结果 → 审核 Agent → 通过/打回
```

适合：需要质量把控的场景

---

## 六、常用 Skill 实战

Skill 是 OpenClaw 的扩展机制，每个 Skill 是一个 `SKILL.md` 文件，教 Agent 如何使用特定工具。

### 6.1 邮件管理（himalaya）

#### 安装配置

```bash
# himalaya 已内置于 OpenClaw 工具链
# 配置文件：~/.config/himalaya/config.toml
```

配置示例（QQ 邮箱）：

```toml
[accounts.default]
default = true
display-name = "你的名字"
email = "your@qq.com"

backend.type = "imap"
backend.host = "imap.qq.com"
backend.port = 993
backend.encryption = "tls"
backend.auth.type = "password"
backend.auth.raw = "your_imap_authorization_code"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.qq.com"
message.send.backend.port = 465
message.send.backend.encryption = "tls"
message.send.backend.auth.type = "password"
message.send.backend.auth.raw = "your_smtp_authorization_code"
```

#### 使用方式

Agent 可以通过 himalaya 命令行完成：

```bash
# 查看最新邮件
himalaya envelope list --page-size 5

# 读取邮件
himalaya message read <id>

# 发送邮件（注意中文编码）
# PowerShell 中必须用文件输入，不能用管道
$proc = Start-Process -FilePath "himalaya.exe" `
  -ArgumentList "template send" `
  -RedirectStandardInput "email.txt" `
  -NoNewWindow -Wait -PassThru

# 搜索邮件
himalaya envelope list -q "UNSEEN"
```

#### 实际场景

```
你：帮我看看有没有新邮件
Agent：你有 3 封未读邮件：
1. [GitHub] PR #123 已合并 (2分钟前)
2. [飞书] 张三 @你了 (15分钟前)
3. [Newsletter] 本周技术周报 (1小时前)

你：回复第一封，说"收到，谢谢"
Agent：已回复 GitHub 通知邮件。
```

### 6.2 日程管理（gog - Google Workspace）

#### 安装配置

gog 是 OpenClaw 的 Google Workspace CLI 工具，支持 Gmail、Calendar、Drive、Docs 等。

```bash
# 授权 Google 账号
gog auth login
```

#### 使用场景

```bash
# 查看今天日程
gog calendar list --today

# 创建会议
gog calendar create \
  --title "项目评审" \
  --start "2026-05-04T14:00:00+08:00" \
  --end "2026-05-04T15:00:00+08:00" \
  --attendees "zhang@company.com,li@company.com"

# 查看 Google Drive 文件
gog drive list --folder "项目文档"
```

#### 实际场景

```
你：明天有什么安排？
Agent：明天（5月4日）你有：
- 10:00-11:00 团队周会（会议室A）
- 14:00-15:00 项目评审（待确认）
- 18:00 和老王吃饭

你：14点的会议推迟到后天同时段
Agent：已将"项目评审"推迟到 5月6日 14:00-15:00，已通知参会人。
```

### 6.3 GitHub 项目管理

Agent 可以通过 `gh` CLI（GitHub CLI）或直接调用 GitHub API 管理项目。

#### 常用操作

```bash
# 查看 PR 列表
gh pr list --repo your-org/your-repo

# 创建 Issue
gh issue create --repo your-org/your-repo \
  --title "修复登录页面样式问题" \
  --body "描述..." \
  --label "bug,frontend"

# 审查 PR
gh pr view 123 --repo your-org/your-repo
gh pr review 123 --approve --body "LGTM"

# 查看 Actions 运行状态
gh run list --repo your-org/your-repo
```

#### 实际场景

```
你：看看 my-project 有什么待处理的 PR
Agent：有 2 个 open PR：
#142 [feat] 新增用户导出功能 (by @zhangsan, 2天前)
  - CI 通过 ✅，2 个 review 待审
#140 [fix] 修复超时问题 (by @lisi, 5天前)
  - CI 通过 ✅，已approve 1人

你：approve #140
Agent：已 approve PR #140，评论：LGTM, nice fix!
```

### 6.4 天气查询

OpenClaw 内置天气 Skill，无需额外配置：

```
你：今天上海天气怎么样？
Agent：上海今天：多云转晴，22-28°C，东南风3级，空气质量良。适合户外活动。
```

### 6.5 网页搜索与信息收集

OpenClaw 支持多种搜索引擎（Brave、DuckDuckGo、Tavily 等）：

```json5
{
  plugins: {
    entries: {
      brave: {
        enabled: true,
        config: {
          webSearch: {
            apiKey: "your_brave_api_key",
          },
        },
      },
    },
  },
}
```

#### 实际场景

```
你：帮我查一下 Next.js 15 有什么新特性
Agent：（搜索后总结）Next.js 15 主要更新：
1. React 19 支持（Server Components 默认启用）
2. Turbopack 稳定版（构建速度提升 76%）
3. 新增 `after()` API（响应后执行逻辑）
4. ...
```

### 6.6 定时任务与心跳

#### Cron 定时任务

```bash
# 创建一次性提醒
openclaw cron add \
  --name "下午茶提醒" \
  --at "2026-05-03T15:00:00+08:00" \
  --session main \
  --system-event "提醒：该喝下午茶了！" \
  --wake now \
  --delete-after-run

# 创建周期任务（每天早上9点检查邮件）
openclaw cron add \
  --name "早间邮件检查" \
  --cron "0 9 * * *" \
  --session main \
  --system-event "检查今日未读邮件并汇报" \
  --tz "Asia/Shanghai"

# 查看所有任务
openclaw cron list
```

#### 心跳机制

在 `HEARTBEAT.md` 中配置定期检查任务：

```markdown
# 心跳任务

## 邮件检查
- 检查新邮件（`himalaya envelope list --page-size 5`）
- 发现新邮件后总结，通过飞书发送提醒
- 每次心跳最多提醒 3 封

## 日历检查
- 检查未来 24 小时内的日程
- 有即将到来的事件时提醒
```

Agent 会定期执行心跳任务，主动推送重要信息。

---

## 七、实战案例：我的 OpenClaw 工作流

### 7.1 日常工作流

```
早上 9:00  → Agent 自动检查邮件和日历，推送今日摘要
工作中      → 飞书群里 @Agent 查代码、写文档、查资料
下午 5:00  → Agent 自动总结今日工作，更新待办
晚上 8:00  → Agent 检查 GitHub PR 状态，提醒 review
```

### 7.2 团队协作流

```
龙豪（秘书 Agent）：
  - 接收殷瑞指令
  - 安全评估
  - 拆解任务分配给钳光
  - 审核结果后汇报

钳光（工程师 Agent）：
  - 接收龙豪指令
  - 执行具体任务（写代码、查日志、部署）
  - 向龙豪汇报结果
```

### 7.3 自动化流

```
PR 合并 → GitHub Webhook → Agent 收到通知 → 自动部署到测试环境 → 飞书通知团队
```

---

## 八、进阶配置

### 8.1 安全配置

```json5
{
  channels: {
    feishu: {
      dmPolicy: "allowlist",       // 私聊白名单
      allowFrom: ["ou_xxx"],       // 只允许指定用户
      groupPolicy: "allowlist",    // 群聊白名单
      groupAllowFrom: ["oc_xxx"],  // 只允许指定群
    },
  },
  tools: {
    exec: {
      security: "allowlist",  // exec 命令白名单模式
    },
  },
}
```

### 8.2 模型配置

```json5
{
  agents: {
    defaults: {
      model: "claude-sonnet-4-20250514",      // 默认模型
      // 支持的提供商：
      // Anthropic (Claude)、OpenAI (GPT)、Google (Gemini)
      // 以及 35+ 其他提供商
    },
    list: [
      {
        id: "main",
        model: "claude-sonnet-4-20250514",  // Agent 专属模型
      },
      {
        id: "fast",
        model: "gpt-4o-mini",  // 轻量任务用便宜模型
      },
    ],
  },
}
```

### 8.3 沙箱配置

```json5
{
  gateway: {
    sandbox: {
      enabled: true,           // 启用沙箱
      network: "restricted",   // 限制网络访问
    },
  },
}
```

### 8.4 远程访问

通过 Tailscale 实现远程访问：

```bash
# 安装 Tailscale
# 配置 Gateway 绑定到 Tailscale 网络
openclaw config set gateway.bind "0.0.0.0"
```

---

## 九、常见问题

### Q1: API Key 怎么选？

| 场景 | 推荐模型 | 提供商 |
|------|----------|--------|
| 通用对话 | Claude 3.5 Sonnet | Anthropic |
| 代码生成 | Claude 3.5 Sonnet / GPT-4o | Anthropic / OpenAI |
| 轻量任务 | GPT-4o-mini | OpenAI |
| 预算有限 | Gemini 1.5 Flash | Google |

### Q2: 网关挂了怎么办？

```bash
# 检查状态
openclaw gateway status

# 重启
openclaw gateway restart

# 查看日志
openclaw logs --tail 50
```

建议配置看门狗脚本，自动监控和恢复。

### Q3: 多人共用一个网关安全吗？

安全。每个 Agent 有独立的工作空间和会话存储，互不访问。通过 `bindings` 将不同渠道/用户路由到不同 Agent。

### Q4: 费用怎么算？

OpenClaw 本身免费开源。费用来自：
- **模型 API 费用**：按调用量计费，不同模型价格不同
- **服务器费用**：如果部署在云服务器上
- **可选服务**：搜索引擎 API、TTS 等

### Q5: Windows 上有什么注意事项？

- 推荐使用 PowerShell 7+
- 中文邮件发送必须用文件输入，不能用管道（编码问题）
- 路径使用反斜杠（`C:\path\to\file`）

---

## 十、总结

OpenClaw 不只是一个 AI 聊天工具，而是一个 **AI Agent 操作系统**。它让你：

1. **统一入口**：一个 Agent，多渠道响应
2. **真正行动**：不只聊天，还能操作文件、发邮件、管日程
3. **团队协作**：多 Agent 分工，各司其职
4. **完全掌控**：数据在你手里，规则你定

开始搭建你的 AI 助理帝国吧。

---

**相关资源：**
- [OpenClaw 官方文档](https://docs.openclaw.ai)
- [GitHub 仓库](https://github.com/openclaw/openclaw)
- [社区 Discord](https://discord.com/invite/clawd)
- [Skill 市场](https://clawhub.ai)
