---
title: Claude Code 完整使用流程指南
description: 从安装到精通的完整教程，涵盖基础使用、进阶功能、多代理系统、Desktop 协调和团队协作
tags:
  - Claude Code
  - AI 开发工具
  - 工作流自动化
  - 多代理协调
author: Hugo
date: 2025-12-07
order: 10
---

# Claude Code 完整使用流程指南

::: info 🎯 学习目标
本指南涵盖 Claude Code 从安装配置到高级多代理工作流的完整使用流程，包括 Agent Skills、Desktop 多代理协调、Git Worktrees 并行开发等新特性，助你将 AI 从简单的代码生成工具转变为可编排的智能开发团队。
:::

---

## 一、🚀 初始安装与配置

### 1.1 安装 Claude Code

```bash
# 安装 Claude Code CLI（避免使用 sudo）
npm install -g @anthropic-ai/claude-code

# 启动 Claude Code
claude
```

### 1.2 初始化配置

1. **选择主题** — 选择终端主题（如深色模式）
2. **身份认证** — 方式一：使用 Claude 订阅账户（推荐，固定月费）；方式二：使用 API Key（按使用量计费）
3. **终端设置** — 选择推荐的默认设置或自定义
4. **工作区信任** — 只在项目目录中运行，避免从根目录启动

### 1.3 定价方案选择

| 方案 | 价格 | 特点 | 适用人群 |
|------|------|------|----------|
| **Pro** | $17-20/月 | 仅支持 Sonnet 模型 | 个人开发者 |
| **Max** | 起步 $100/月 | Sonnet + Opus，5倍使用量 | 专业开发 |

::: tip 💡 模型选择建议
- **Sonnet** — 日常编码任务（快速、高效、成本友好）
- **Opus** — 复杂推理、深度规划、研究任务
:::

---

## 二、🎯 基础使用流程

### 2.1 项目初始化

```bash
# 1. 进入项目目录
cd /path/to/your/project

# 2. 启动 Claude Code
claude

# 3. 初始化项目上下文
/init
```

::: info 📌 `/init` 命令的作用
- 自动分析整个代码库
- 生成 `CLAUDE.md` 文件（项目记忆文件）
- 包含项目架构、技术栈、关键文件、开发命令等信息
:::

### 2.2 构建项目记忆系统

**记忆层级结构：**

```text
~/.claude/CLAUDE.md          # 用户级记忆（全局偏好）
./CLAUDE.md                   # 项目级记忆（团队共享）
./memory/spec/CLAUDE.md       # 自定义模块化记忆
./memory/frontend/CLAUDE.md   # 专门领域记忆
```

**添加记忆的方法：**

| 方法 | 命令 | 说明 |
|------|------|------|
| 快捷键 | `内容 #` | 在句尾加 `#` 快速添加新记忆 |
| 命令 | `/memory` | 打开记忆文件编辑器 |
| 手动 | `mkdir -p memory/frontend` | 创建模块化记忆目录 |

### 2.3 基本交互模式

**直接对话模式（快速任务）：**

适用于简单问题、快速修复、单文件编辑：

- "修复 `src/utils.ts` 中的 TypeScript 错误"
- "给 `calculateTotal` 函数添加注释"
- "这个项目使用什么技术栈？"

**规范驱动开发模式（复杂任务）：**

1. **切换到计划模式** — 按 <kbd>Shift</kbd> + <kbd>Tab</kbd> 或在提示中明确说明"使用计划模式"
2. **创建项目规范** — 描述功能需求，Claude 会使用网络搜索收集信息
3. **保存规范** — 将规范保存到 `memory/spec/CLAUDE.md`
4. **执行实现** — 使用 `根据 @memory/spec/CLAUDE.md 实现主页网格布局`

---

## 三、⚙️ 进阶功能配置

### 3.1 斜杠命令管理

**上下文管理命令：**

| 命令 | 作用 | 使用时机 |
|------|------|----------|
| `/clear` | 清除对话历史（释放上下文空间） | 开始新任务时 |
| `/compact` | 压缩对话历史（保留关键信息） | 上下文过载时 |

**配置与成本命令：**

| 命令 | 作用 |
|------|------|
| `/config` | 打开配置面板（用户/项目/本地三级配置） |
| `/cost` | 查看当前会话成本和持续时间 |
| `/memory` | 编辑记忆文件 |

**代理与工具管理：**

| 命令 | 作用 |
|------|------|
| `/agents` | 管理子代理（创建、编辑、删除） |
| `/mcp` | 管理 MCP 服务器（外部工具集成） |
| `/hooks` | 管理钩子（自动化工作流） |
| `/ide` | 安装 IDE 集成（Cursor/VS Code） |

### 3.2 MCP 集成（外部工具）

**添加 MCP 服务器：**

```bash
# 示例：添加 Context7（30,000+ 库的最新文档）
claude mcp add --transport http context7 https://mcp.context7.com/mcp --scope project

# 重启 Claude Code 生效
/exit
claude
```

**使用 MCP 工具：**

::: tip 💡 自动使用（通过记忆规则）

在 `CLAUDE.md` 中添加：

```text
每次我询问关于 LangGraph 的问题时，自动使用 context7 MCP
```

:::

手动调用：`"使用 context7 查询 Next.js 14 的最新路由功能"`

### 3.3 自定义斜杠命令

**文件位置：** `.claude/commands/commit-code.md`

```yaml
---
name: commit-code
description: 生成智能 Git 提交信息
---

# 系统提示
分析 git diff，生成简洁的提交信息。
使用用户提供的提示作为主题：$arguments

# 示例
用户输入：/commit-code 自定义命令功能
输出：feat: 添加自定义命令创建功能
```

::: details 📎 高级命令示例（带安全控制）

**文件位置：** `.claude/commands/smart-commit.md`

```yaml
---
name: smart-commit
description: 带 RAG 的智能提交
allowed-tools:
  - git status
  - git diff
  - git add
  - git commit
---

# 工作流程
1. 运行 `!git status` 获取当前状态
2. 运行 `!git diff` 查看更改
3. 运行 `!git log -5 --oneline` 分析历史风格
4. 生成提交信息
5. 运行 `!git add .` 和 `!git commit -m "[生成的信息]"`
```

💡 **说明：** `allowed-tools` 实现最小权限原则

:::

### 3.4 输出样式自定义

**创建输出样式：**

```bash
# 用户级样式（全局可用）
/output-style:new
提示："使用简洁的项目符号，精炼要点"

# 项目级样式（项目专属）
/output-style:new
提示："项目级样式，所有响应格式化为 YAML，包含状态、下一步、风险字段"
```

**样式配置文件位置：**

- 用户级：`~/.claude/output-styles/minimal-bullets.md`
- 项目级：`./.claude/output-styles/yaml-concise.md`

### 3.5 自定义状态栏

```bash
# 基础状态栏（显示输出样式）
/statusline
提示："显示当前 output-style，使用 Python 实现，通过 uv 运行"

# 高级状态栏（显示最后提示）
/statusline
提示："显示当前会话的最后一个用户提示，读取 transcript_path，过滤命令和 AI 响应"
```

---

## 四、🤖 高级多代理工作流

### 4.1 子代理系统

::: info 📌 子代理的核心概念
- **隔离上下文** — 每个子代理有独立的上下文窗口
- **专用工具** — 可限制子代理的工具访问权限
- **可重用性** — 跨项目和团队共享
:::

**创建子代理：**

```bash
# 启动代理创建流程
/agents

# 选择范围
# - Project（项目级，保存到 .claude/agents/）
# - User（用户级，保存到 ~/.claude/agents/）

# 生成方式
# - Generate with Claude（交互式生成）
# - Manual（手动编辑）
```

**子代理配置文件示例：**

```yaml
---
name: code-comedy-carl
description: 当用户说 "funny review" 时，生成幽默的代码审查
model: sonnet
color: yellow
tools:
  - read_file
  - grep
  - list_dir
---

# 系统提示
你是 Code Comedy Carl，一位以幽默方式审查代码的专家。

# 审查流程
1. 读取文件
2. 分析代码质量
3. 生成幽默但有建设性的反馈
4. 包含实际改进建议
```

**调用子代理：**

| 方式 | 示例 |
|------|------|
| 单次调用 | `"funny review @main.py"` |
| 批量调用 | `"创建 2 个有趣的代码审查"` |

**常用子代理类型：**

| 类型 | 描述 | 工具 |
|------|------|------|
| **代码审查员** | 执行彻底的代码审查，检查质量、安全性、性能 | `read_file`, `grep`, `codebase_search` |
| **研究员** | 进行深度技术研究，查找文档和最佳实践 | `web_search`, `mcp_context7` |
| **测试代理** | 生成测试用例并运行测试 | `read_file`, `write`, `run_terminal_cmd` |
| **图表生成器** | 将文本概念转换为 Mermaid 流程图 | `web_search`, `write` |

### 4.2 并行会话（多 Claude 实例）

::: warning ⚠️ 使用场景
**✅ 适用任务（独立任务）：**
- 修复不同模块的不相关 bug
- 构建独立的 UI 页面（关于页面 + 联系页面）
- 创建独立的组件（按钮组件 + 模态框组件）

**❌ 避免任务（依赖任务）：**
- 一个代理构建 API，另一个构建调用该 API 的前端
- 相互依赖的功能（会导致契约分歧和静默失败）
:::

**实施步骤：**

```bash
# 终端 1
cd /path/to/project
claude
# 任务："重新设计 HookCard.tsx 组件，使其更现代、视觉吸引"

# 终端 2（同一目录）
cd /path/to/project
claude
# 任务："重新设计 page.tsx 中的 hero 区域"
```

> 💡 **角色转变：** 开发者从 **编写代码** 转变为 **编排 AI 代理** — 识别可并行化的任务、确定任务依赖关系、协调多个 AI 代理的工作

### 4.3 专门化 AI 环境

**挑战：** 同一目录的多个实例共享配置（输出样式会相互影响）

**解决方案：** 为每个专门代理创建独立目录

```bash
# 创建专门化工作区
mkdir -p ~/ai-agents/frontend-expert
mkdir -p ~/ai-agents/backend-expert
mkdir -p ~/ai-agents/devops-expert

# 在每个目录中配置独立的输出样式和记忆
cd ~/ai-agents/frontend-expert
claude
/output-style:new "项目级，专注于 React 最佳实践"
```

::: tip 💡 未来愿景
每个终端窗口 = 一个命名的 AI 专家：
- `Frontend Expert` — React/Next.js 专家，详细的组件建议
- `Backend Expert` — API 设计，YAML 格式输出
- `DevOps Expert` — 基础设施即代码，Terraform/Docker 专家
- `Security Auditor` — 安全审查，CVE 扫描
:::

### 4.4 Agent Skills（代理技能）

::: info 📌 什么是 Agent Skills？
Agent Skills 是**程序化知识容器**——包含指令和脚本的文件夹，用于教代理如何一致地执行特定任务。Skills 是 Claude Code 的五大 Agent 系统原语之一。
:::

**Agent 系统原语：**

| 原语 | 用途 | 特点 |
|------|------|------|
| **Memories** | 持久化上下文 | 自动加载 |
| **Slash Commands** | 快速触发操作 | 用户驱动 |
| **Skills** | 程序化知识 | 渐进式加载 |
| **Subagents** | 隔离的专门助手 | 独立上下文 |
| **MCP Servers** | 外部资源连接 | 服务器执行 |

**Skills 的核心优势——渐进式加载（Progressive Disclosure）：**

- 初始时只加载 Skill 的头部信息（~100 tokens）
- 完整指令仅在代理决定使用该 Skill 时才加载（~2K tokens）
- 极其高效的上下文利用

**创建自定义 Skill：**

**目录结构：**

```text
.claude/skills/
└── git-pushing/
    ├── SKILL.md           # Skill 定义文件
    └── smart_commit.sh    # 辅助脚本
```

**SKILL.md 示例：**

```yaml
---
name: Git Smart Push
description: Automatically generate commit messages and push to remote
---

# Git Smart Push Skill

This skill handles git operations with intelligent commit message generation.

## Execution
!bash .claude/skills/git-pushing/smart_commit.sh $arguments
```

**Skills vs MCP vs Subagents 对比：**

| 维度 | Skills | MCP | Subagents |
|------|--------|-----|-----------|
| **上下文消耗** | 极低（渐进式） | 较高（预先定义） | 独立窗口 |
| **执行位置** | 本地，主代理线程 | 服务器（本地/云端） | 隔离上下文 |
| **主要用途** | 一致的方法论 | 外部资源连接 | 长周期复杂任务 |
| **灵活性** | 较低 | 中等 | 较高 |

::: tip 💡 选择建议
- **Skills** — 需要一致方法论、低上下文开销的短任务
- **MCP** — 需要连接外部 API、数据库、浏览器自动化
- **Subagents** — 重度任务、需要隔离上下文、长周期任务
:::

---

## 五、🖥️ Claude Code Desktop 与多代理协调

### 5.1 Desktop 运行模式

Claude Code Desktop 提供两种截然不同的运行模式：

| 模式 | 特点 | 执行环境 | 适用场景 |
|------|------|----------|----------|
| **Local Worktree** | 本地执行，Git Worktree | 本地机器 | 日常开发、完全控制 |
| **Default (Cloud)** | 云端容器，GitHub 集成 | Anthropic 服务器 | 并行任务、无限扩展 |

**Local Worktree 模式：**

```text
你的项目目录 (/project)
    ├── .git/
    ├── src/
    └── ...
         │
         │ Claude Desktop 创建 Worktree
         ▼
worktree 目录 (/zealous-jemison)
    ├── src/  (独立分支)
    └── ...
    
    Claude 在此独立工作
```

**Default (Cloud) 模式：**

```text
1. 授权 Anthropic 访问 GitHub
2. 云代理克隆仓库到容器
3. 执行代码工作
4. 推送提交或创建 PR
5. 像远程人类开发者一样工作
```

### 5.2 Git Worktrees 并行开发

::: info 📌 为什么使用 Git Worktrees？
Git Worktrees 是多代理并行工作的"协调成本"——允许多个代理同时在不同分支工作，而不会相互干扰。
:::

**常用命令：**

```bash
# 创建新的 worktree（基于新分支）
git worktree add ../feature-animation feature-animation

# 创建 worktree（基于远程分支）
git worktree add ../hotfix origin/hotfix -b hotfix

# 列出所有 worktrees
git worktree list

# 删除 worktree
git worktree remove ../feature-animation

# 清理已删除 worktree 的引用
git worktree prune
```

**优势：**

- ✅ **隔离性** — 代码更改在独立目录，主分支完全不受影响
- ✅ **并行工作** — 多代理可同时处理不同分支
- ✅ **干净工作流** — 无需 `git stash` 或多次克隆仓库
- ✅ **灵活性** — 结果好则合并，不好则删除 worktree

### 5.3 三重并行开发工作流

**同时运行三个独立的开发工作流：**

| 任务 | 模式 | 工作内容 |
|------|------|----------|
| **任务 A** | Claude Chat（云模型） | 研究：搜索 GitHub hooks 仓库 |
| **任务 B** | Local Worktree | 功能开发：添加 UI 动画 |
| **任务 C** | Default（云端） | 远程开发：更新 Hero 区域 |

**协调流程：**

```bash
# 任务 A: 在 Claude Desktop 聊天界面
找出最流行的 Claude Code hooks 开源仓库

# 任务 B: 在 Local Worktree 模式
# Claude 自动创建 worktree（如 zealous-jemison/）
为主页添加入场动画效果

# 任务 C: 在 Default（云端）模式
在 project/hookhub 分支上更新 Hero 区域
```

### 5.4 合并多代理工作

**步骤 1: 提交各自的工作**

```bash
# 在每个 worktree 中提交
cd ~/worktrees/zealous-jemison
git add . && git commit -m "feat: add entrance animations"

cd ~/worktrees/vigilant-feistel
git add . && git commit -m "feat: update hooks database"
```

**步骤 2: 推送到 GitHub**

```bash
git push origin zealous-jemison
git push origin vigilant-feistel
```

**步骤 3: 让 Claude 执行合并**

```text
将以下分支合并到 project/hookhub：
- zealous-jemison (动画功能)
- vigilant-feistel (数据库更新)
- anthropic-hero-design-xxx (云端 hero 更新)

请：
1. 检出 project/hookhub
2. 依次合并这些分支
3. 解决任何冲突
4. 运行测试验证
```

**步骤 4: 验证并推送**

```bash
npm run dev  # 验证
git push origin project/hookhub  # 推送
```

### 5.5 Claude Code Mobile

::: info 📌 移动端特点
- 通过 Claude iOS/Android 应用使用
- 强制使用 Default（云容器）模式
- 适合简单任务和紧急修复
:::

**移动端工作流：**

```text
📱 移动端发起任务
    ↓ (云容器执行)
    ↓ 克隆仓库 → 检出分支 → 执行任务 → 提交更改
    ↓
创建 PR / 推送代码
    ↓
💻 桌面端验证和修复
    ↓ 打开 PR → 检查分支 → 本地拉取 → 运行验证
```

::: warning ⚠️ 注意事项
- 默认云环境没有本地 MCP 或自定义 hooks
- PR 可能默认指向错误的基础分支，需手动检查
:::

---

## 六、🔗 团队协作与自动化

### 6.1 GitHub Actions 自动化

**前置条件：**

```bash
# 1. 安装 GitHub CLI
brew install gh  # macOS

# 2. 认证 GitHub CLI
gh auth login

# 3. 确保在 Git 仓库目录中
cd /path/to/your/repo
```

**安装 Claude GitHub App：**

```bash
# 在 Claude Code 中运行
/install-github-app

# 按照提示操作：
# 1. 在浏览器中授权 Claude GitHub App
# 2. 选择要集成的仓库
# 3. 选择工作流（@Claude issue 评论、自动代码审查）
# 4. 选择认证方式（使用订阅绑定的 token）
```

安装会自动创建 PR，添加以下文件：

```text
.github/workflows/claude-issue-comment.yml
.github/workflows/claude-pr-review.yml
```

**使用方式：**

| 触发方式 | 示例 |
|----------|------|
| Issue 评论 | 在 GitHub Issue 中评论 `@claude 能否修复这个 bug？` |
| PR 评论 | 在 Pull Request 中评论 `@claude 请审查这个 PR` |

::: tip 💡 最佳实践：提供上下文

在仓库中添加 `CLAUDE.md`，Claude 在执行 GitHub Actions 时会读取它，理解项目架构和约束。

```bash
# 在本地项目中
claude
/init  # 生成 CLAUDE.md

# 提交并推送
git add CLAUDE.md
git commit -m "docs: 添加 Claude AI 项目上下文"
git push
```

:::

### 6.2 IDE 集成（Cursor）

```bash
# 在 Claude Code 中运行
/ide

# 选择 Cursor（或 VS Code）
# 自动安装扩展
```

**优势：**

- 自动设置工作目录
- 访问打开的文件
- 上下文感知（光标位置、选中文本）

### 6.3 钩子（Hooks）自动化

**钩子类型：**

| 时机 | 说明 |
|------|------|
| `before_tool_use` | 工具使用前 |
| `after_tool_use` | 工具使用后 |
| `on_start` | Claude 启动时 |
| `on_error` | 错误发生时 |

**示例配置（`.claude/hooks.json`）：**

```json
{
  "after_tool_use": {
    "edit": "prettier --write $FILE && eslint --fix $FILE",
    "write": "prettier --write $FILE"
  },
  "before_tool_use": {
    "delete": "echo '确认删除 $FILE' && read -p '继续? (y/n) ' confirm"
  }
}
```

::: details 📎 高级用法：调用子代理

```json
{
  "after_tool_use": {
    "write": "claude invoke code-reviewer @$FILE"
  }
}
```

:::

---

## 七、✅ 最佳实践与注意事项

### 7.1 上下文工程原则

**分层记忆策略：**

```text
~/.claude/CLAUDE.md          # 个人偏好、代码风格
./CLAUDE.md                   # 项目架构、团队规范
./memory/spec/CLAUDE.md       # 功能规范
./memory/frontend/CLAUDE.md   # 领域知识（按需加载）
```

**原则：**

- 保持记忆文件简洁（避免过载上下文）
- 使用模块化目录结构
- 通过 `@` 语法手动加载特定记忆

**动态上下文管理：**

| 命令 | 使用时机 | 效果 |
|------|----------|------|
| `/compact` | 长对话时 | 压缩历史，保留关键决策和信息，节省 token |
| `/clear` | 开始新任务时 | 释放完整上下文空间，保留项目记忆 |

::: warning ⚠️ 避免上下文污染
- **上下文中毒** — 早期错误污染后续流程
- **上下文混淆** — 无关信息分散注意力
- **上下文冲突** — 矛盾信息导致困惑

**解决方案：** 使用子代理隔离复杂任务、定期 `/compact` 或 `/clear`、精确定义记忆内容

:::

### 7.2 安全性最佳实践

**最小权限原则：**

自定义命令：

```yaml
---
allowed-tools:
  - git status
  - git diff
  # 只授予必要的工具
---
```

子代理：

```yaml
---
tools:
  - read_file
  - grep
  # 不授予 write 或 run_terminal_cmd
---
```

**工作区信任：**

- 始终在项目目录中运行 `claude`
- 避免从根目录或敏感目录启动
- 定期审查 Claude 的操作

### 7.3 成本优化策略

**模型选择：**

| 任务类型 | 推荐模型 | 原因 |
|----------|----------|------|
| 日常编码 | Sonnet | 快速、成本低、质量高 |
| 复杂规划 | Opus | 深度推理、研究能力强 |
| 简单问答 | Sonnet | 足够智能、响应快 |

**Token 节省技巧：**

- 使用 `/compact` 压缩历史
- 避免重复读取大文件
- 使用子代理隔离大型任务（子代理的 token 不计入主代理）
- 查看当前会话成本：`/cost`

### 7.4 工作流程最佳实践

::: details 📎 典型的一天工作流

**早晨（项目启动）：**

```bash
cd ~/projects/my-app
claude
/memory  # 检查项目状态
"总结昨天的工作"  # 如果需要
```

**日间（功能开发）：**

```bash
# 复杂功能：使用规范驱动
Shift+Tab  # 进入计划模式
"帮我规划用户认证系统的实现"
# 审查计划 → 批准 → 执行

# 简单任务：直接实现
"修复 UserProfile.tsx 中的 TypeScript 错误"
```

**代码审查：**

```bash
"code review @src/auth/login.ts"
```

**提交代码：**

```bash
/commit-code 用户认证功能
```

**傍晚（清理和总结）：**

```bash
/compact  # 压缩对话历史
"今天实现了 JWT 认证，使用 jose 库而非 jsonwebtoken #"  # 添加重要记忆
```

:::

::: details 📎 团队协作流程

**项目初始化（团队负责人）：**

```bash
# 1. 创建项目记忆
/init

# 2. 添加团队规范（/memory）
# - 代码风格指南
# - Git 提交规范
# - 架构决策
# - 第三方依赖说明

# 3. 创建共享子代理
/agents
# 创建项目级代理：code-reviewer、test-generator

# 4. 配置 MCP 和钩子
claude mcp add context7 --scope project
/hooks  # 配置自动格式化

# 5. 提交配置文件
git add .claude/ CLAUDE.md
git commit -m "chore: 配置 Claude Code 团队环境"
git push
```

**团队成员使用：**

```bash
git clone <repo-url>
cd <project>
claude  # 自动加载团队配置
"根据 @CLAUDE.md 实现用户注册功能"
```

:::

### 7.5 故障排查

| 问题 | 解决方案 |
|------|----------|
| **权限错误（EACCES）** | `sudo chown -R $(whoami) ~/.npm`（修复 npm 目录所有权） |
| **MCP 未加载** | 检查配置 `/mcp` → 重启 Claude `/exit && claude` → 授予权限 |
| **子代理未触发** | 检查文件位置（`.claude/agents/`）、`description` 字段清晰、提示词包含触发短语 |
| **状态栏显示错误** | 查看错误信息 → 请求修复 → 手动编辑 `~/.claude/statusline.py` |

---

## 八、📖 快速参考卡片

### 8.1 常用命令速查

| 命令 | 用途 | 使用频率 |
|------|------|----------|
| `/init` | 初始化项目记忆 | 项目开始时 |
| `/clear` | 清除对话历史 | 切换任务时 |
| `/compact` | 压缩对话历史 | 对话过长时 |
| `/memory` | 编辑记忆文件 | 需要添加规则时 |
| `/agents` | 管理子代理 | 创建专门代理时 |
| `/mcp` | 管理 MCP 服务器 | 添加外部工具时 |
| `/cost` | 查看成本 | 监控使用量时 |
| `/output-style:new` | 创建输出样式 | 自定义交互方式时 |

### 8.2 提示词模板

| 场景 | 模板 |
|------|------|
| 功能实现 | `根据 @memory/spec/CLAUDE.md 实现 [功能名称]` |
| 代码审查 | `code review @[文件路径]` |
| 调试 | `调试 @[文件路径] 中的 [问题描述]` |
| 重构 | `重构 @[文件路径]，改进 [具体方面]` |
| 文档生成 | `为 @[文件路径] 生成详细的文档` |

### 8.3 上下文引用语法

| 语法 | 作用 |
|------|------|
| `@文件路径` | 引用特定文件 |
| `@目录路径` | 引用整个目录 |
| `@memory/spec/CLAUDE.md` | 引用特定记忆文件 |
| `#` | 快速添加记忆（在句尾） |

### 8.4 Git Worktrees 速查

| 命令 | 用途 |
|------|------|
| `git worktree add ../dir branch` | 创建新 worktree |
| `git worktree list` | 列出所有 worktrees |
| `git worktree remove ../dir` | 删除 worktree |
| `git worktree prune` | 清理引用 |

### 8.5 Desktop 模式对比

| 特性 | Local Worktree | Cloud (Default) |
|------|----------------|-----------------|
| 执行环境 | 本地机器 | Anthropic 服务器 |
| MCP/Hooks | ✅ 可用 | ❌ 需另配置 |
| 扩展性 | 受限本地资源 | 无限（预算允许） |
| 结果交付 | 本地 Git 分支 | GitHub PR/提交 |

---

## 九、🎓 核心概念总结

### 9.1 上下文工程（Context Engineering）

> 在任务的每个步骤中，用正确的信息填充 LLM 的上下文窗口的艺术和科学

**关键要素：**

- **持久记忆** — CLAUDE.md 文件
- **智能检索** — 自动上下文发现
- **上下文压缩** — `/compact` 命令
- **上下文隔离** — 子代理系统

### 9.2 规范驱动开发（Spec-Driven Development）

> 先规划（计划模式）→ 后执行（实现模式）

**优势：**

- ✅ 更安全（先审查再执行）
- ✅ 更一致（明确规范）
- ✅ 可追溯（规范文档可共享）

### 9.3 多代理编排（Multi-Agent Orchestration）

> 开发者从编码员转变为 AI 代理的指挥家

**核心能力：**

- 识别可并行化的任务
- 分配专门化角色
- 协调代理协作
- 管理依赖关系

---

### 9.4 Agent Skills 与 Desktop 协调

> 从单代理到多代理协调的进化

**核心能力：**

- 理解 Skills vs MCP vs Subagents 的权衡
- 掌握 Desktop 两种运行模式
- 使用 Git Worktrees 实现并行开发
- 协调本地和云端代理协同工作

---

## 十、📚 进阶学习路径

### 第一阶段：基础掌握（1-2 周）

- [x] 安装和配置
- [x] `/init` 创建项目记忆
- [x] 基本对话和代码生成
- [x] 使用 `/clear` 和 `/compact` 管理上下文

### 第二阶段：进阶功能（2-4 周）

- [x] 创建自定义斜杠命令
- [x] 配置输出样式
- [x] 集成 MCP 服务器
- [x] 使用计划模式进行规范驱动开发

### 第三阶段：多代理系统（4-8 周）

- [x] 创建和配置子代理
- [x] 并行会话工作流
- [x] 配置钩子自动化
- [x] 设置 GitHub Actions 集成

### 第四阶段：Agent Skills 与 Desktop（8-12 周）

- [x] 理解 Agent Skills 和渐进式加载
- [x] 创建自定义 Skills（带辅助脚本）
- [x] 掌握 Desktop 两种运行模式
- [x] Git Worktrees 并行开发
- [x] 多代理协调与合并工作流
- [x] Mobile 端开发流程

### 第五阶段：团队和企业（持续优化）

- [x] 团队配置标准化
- [x] 企业级安全策略
- [x] 自定义工作流优化
- [x] 性能和成本监控

---

<div style="text-align: center; padding: 20px; background: var(--vp-c-bg-soft); border-radius: 8px; margin: 24px 0;">

**最终目标：将 AI 从简单的代码生成工具转变为可编排的智能开发团队。**

</div>

---

**文档版本**：v2.0 | **最后更新**：2025-12-07 | **适用版本**：Claude Code CLI + Desktop
