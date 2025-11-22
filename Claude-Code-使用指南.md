# Claude Code 完整使用指南

## 目录

1. [快速入门](#快速入门)
2. [核心概念](#核心概念)
3. [安装与配置](#安装与配置)
4. [斜杠命令详解](#斜杠命令详解)
5. [内存系统（Memory）](#内存系统memory)
6. [自定义命令](#自定义命令)
7. [MCP 集成](#mcp-集成)
8. [子代理（Subagents）](#子代理subagents)
9. [输出样式（Output Styles）](#输出样式output-styles)
10. [Hooks 自动化](#hooks-自动化)
11. [GitHub 集成](#github-集成)
12. [高级工作流](#高级工作流)
13. [最佳实践](#最佳实践)

---

## 快速入门

### 安装

**macOS/Linux (Homebrew):**
```bash
brew install claude-code
```

**macOS/Linux (curl):**
```bash
curl -fsSL https://claude.ai/install.sh | sh
```

**Windows (PowerShell):**
```powershell
irm https://claude.ai/install.ps1 | iex
```

**npm:**
```bash
npm install -g @anthropic-ai/claude-code
```

### 首次运行

```bash
claude
```

首次运行时会进行初始化设置：
- 选择主题（深色/浅色）
- 认证方式选择（订阅账号 或 API Key）
- 配置终端设置
- 授权工作区信任

### 基本使用

```bash
# 在项目目录中启动
cd your-project
claude

# 初始化项目（自动分析并创建 CLAUDE.md）
/init

# 开始对话
你: 帮我重构这个函数
```

---

## 核心概念

### 1. Context Engineering（上下文工程）

上下文工程是构建成功 AI 代理的关键，它是在任务的每个步骤中，用正确的信息填充 LLM 上下文窗口的艺术和科学。

**四大核心策略：**

#### Strategy 1: Write Context（持久化内存）
- **项目内存** (`./CLAUDE.md`)：团队共享的项目架构和编码标准
- **用户内存** (`~/.claude/CLAUDE.md`)：跨所有项目的个人偏好
- **动态导入**：使用 `@` 语法模块化加载上下文

#### Strategy 2: Select Context（智能检索）
- 自动发现上下文文件
- 用户驱动的上下文（`#` 快捷键快速添加）
- 工具特定上下文（不同工具自动加载不同上下文）

#### Strategy 3: Compress Context（高效压缩）
- `/clear`：重置对话历史
- `/compact`：压缩对话历史为核心摘要

#### Strategy 4: Isolate Context（多代理系统）
- 专门的子代理处理特定任务
- 每个子代理有独立的上下文
- 主代理作为管理者委托任务

### 2. 上下文相关的常见问题

- **Context Poisoning（上下文污染）**：早期错误污染后续流程
- **Context Confusion（上下文混乱）**：无关信息分散注意力
- **Context Clash（上下文冲突）**：矛盾信息导致混乱

---

## 安装与配置

### 定价模式

**两种计费方式：**

1. **Claude 订阅**（推荐）
   - **Pro 计划** ($17-20/月)：仅限 Sonnet 模型
   - **Max 计划** ($100+/月)：可使用 Opus 和 Sonnet，用量是 Pro 的 5 倍以上

2. **API Key**（按用量计费）
   - 适合测试或偶尔使用
   - 注意：使用 `/cost` 监控成本
   - **重要**：设置月度支出限制以防意外高额账单

### 模型选择

- **Claude Sonnet**：推荐用于日常编码，速度快、性价比高
- **Claude Opus**：用于复杂推理、深度规划和研究任务

### IDE 集成

```bash
# 安装 IDE 扩展（Cursor 或 VS Code）
/ide
```

安装后可直接从 IDE 启动 Claude Code，自动设置正确的工作目录。

---

## 斜杠命令详解

### 会话管理

```bash
/clear          # 清空对话历史，重新开始
/compact        # 压缩对话历史，保留关键信息
/cost           # 查看当前会话的成本和时长
/exit           # 退出 Claude Code
```

### 配置管理

```bash
/config         # 打开配置面板
/memory         # 编辑内存文件（CLAUDE.md）
/agents         # 管理子代理
/mcp            # 管理 MCP 服务器
/hooks          # 管理自动化钩子
```

### 项目初始化

```bash
/init           # 分析项目并自动生成 CLAUDE.md
```

### GitHub 集成

```bash
/install-github-app    # 安装 GitHub App 实现自动化工作流
```

### 自定义功能

```bash
/output-style          # 管理输出样式
/output-style:new      # 创建新的输出样式
/statusline            # 自定义状态栏
```

---

## 内存系统（Memory）

### 内存层级结构

Claude Code 使用分层的内存系统：

```
~/.claude/CLAUDE.md           # 用户全局内存（所有项目共享）
  └── project/CLAUDE.md       # 项目内存（团队共享）
      └── project/memory/     # 专门的上下文目录
          ├── frontend/CLAUDE.md
          ├── backend/CLAUDE.md
          └── spec/CLAUDE.md
```

### 使用方法

**查看/编辑内存：**
```bash
/memory
```

**快速添加内存：**
```bash
# 使用 # 快捷键
我喜欢使用 tabs 而不是 spaces
```

**引用特定内存文件：**
```bash
# 使用 @ 符号引用
根据 @memory/spec/CLAUDE.md 中的规范实现主页网格
```

### 最佳实践

```markdown
<!-- 项目 CLAUDE.md 示例 -->

# 项目概述
这是一个 Next.js 项目，用于展示开源的 Claude hooks。

## 技术栈
- Next.js 14 (App Router)
- TypeScript
- Tailwind CSS

## 编码规范
- 使用 TypeScript 严格模式
- 组件使用函数式写法
- 优先使用 Server Components

## 架构
- `/src/app` - 页面和路由
- `/src/components` - 可复用组件
- `/src/types` - TypeScript 类型定义

## 常用命令
- `npm run dev` - 启动开发服务器
- `npm run build` - 生产构建
```

**重要提示：**
- 保持内存文件简洁明确
- 避免过载 AI，浪费 tokens
- 使用多个 CLAUDE.md 文件分散上下文
- Claude 会递归向上查找并加载所有 CLAUDE.md

---

## 自定义命令

### 基础自定义命令

在 `.claude/commands/` 目录创建 Markdown 文件：

```markdown
<!-- .claude/commands/commit-code.md -->
---
name: commit-code
description: Generate a git commit message
---

# Instructions

Review the staged changes and generate a concise, descriptive commit message.

Use user hints as the message main subject: $arguments
```

**使用：**
```bash
/commit-code 修复登录bug
```

### 高级自定义命令（带 Bash 执行）

```markdown
<!-- .claude/commands/commit-advanced.md -->
---
name: commit-advanced
description: Advanced commit message generator with RAG
allowed-tools:
  - git add
  - git status
  - git commit
  - git diff
  - git branch
  - git log
---

# Instructions

Before generating the commit message, retrieve context using:

!git status
!git diff --staged
!git branch --show-current
!git log -1 --oneline

Then generate a commit message following the Conventional Commits standard.

User hint: $arguments
```

**安全优势：**
- `allowed-tools` 遵循最小权限原则
- 防止未授权命令执行
- 避免上下文污染攻击

### 作用域

- **项目作用域**：`.claude/commands/` （推荐，团队共享）
- **用户作用域**：`~/.claude/commands/` （个人使用）

---

## MCP 集成

### 什么是 MCP？

Model Context Protocol（模型上下文协议）为 Claude Code 提供额外的工具和能力。

### 添加 MCP 服务器

```bash
# 添加远程 MCP（例如：Context7）
claude mcp add --transport http context7 https://mcp.context7.com/mcp

# 指定作用域
claude mcp add --scope project context7 https://mcp.context7.com/mcp
# --scope 选项：
#   project - 项目级（保存到 .mcp.json）
#   user    - 用户级（全局）
#   local   - 会话级（临时）

# 重启 Claude Code 使配置生效
/exit
claude
```

### 使用 MCP

**列出可用 MCP：**
```bash
/mcp
```

**显式使用 MCP：**
```
请使用 context7 MCP 查找 LangGraph 的最新文档
```

**自动使用（通过内存规则）：**
```markdown
<!-- 在 CLAUDE.md 中添加 -->
每次我询问 LangGraph 相关问题时，自动使用 context7 MCP。
```

### 常用 MCP 服务器

- **Context7**：30,000+ 库的最新文档
- **Playwright**：浏览器自动化
- **Database Connectors**：数据库查询和管理

---

## 子代理（Subagents）

### 什么是子代理？

子代理是具有特定专长的预配置 AI 助手，主代理可以委托任务给它们。

**特点：**
- 专门的系统提示
- 独立的上下文窗口（避免污染）
- 可配置特定工具
- 可复用

### 创建子代理

**交互式创建：**
```bash
/agents
# 选择 "Project" 作用域
# 选择 "Generate with Claude"
# 输入描述：创建一个有趣的代码审查员
```

**手动创建：**

在 `.claude/agents/code-reviewer.md` 中：

```markdown
---
name: Code Reviewer
description: Reviews code for bugs, style issues, and improvements
model: claude-sonnet-4-latest
color: yellow
allowed-tools:
  - read
  - grep
  - search
---

# System Prompt

You are an expert code reviewer with a focus on:
- Code quality and maintainability
- Performance optimization
- Security vulnerabilities
- Best practices

## Review Process

1. Read the specified files
2. Analyze the code structure
3. Identify issues and improvements
4. Provide constructive feedback with examples
5. Rate the overall code quality

Be thorough but concise. Prioritize critical issues.
```

### 调用子代理

**方式 1：使用触发短语**
```
请对 @main.py 进行有趣的代码审查
# 如果 description 包含 "funny review"，会自动触发
```

**方式 2：多任务**
```
创建 3 个代码审查报告
# Claude 会启动 3 个子代理实例
```

### 子代理工作原理

```
主代理（历史很长）
  └─> 创建简洁的任务提示
        └─> 子代理（全新上下文）
              - 执行任务（可能用很多 tokens）
              - 使用专门的工具
              - 迭代改进
              └─> 返回简洁的结果
主代理（上下文仅增加简洁结果）
```

**好处：**
- 主上下文保持简洁
- 避免主代理 token 限制
- 提高专注度和准确性

### 实用子代理示例

**Mermaid 图表生成器：**
```markdown
---
name: Mermaid Diagram Generator
description: Converts concepts to mermaid diagrams
model: claude-sonnet-4-latest
color: blue
allowed-tools:
  - web_search
---

# System Prompt

You are a Mermaid diagram expert. Create SIMPLE, clear diagrams.

## Process
1. Search for existing diagram examples online
2. Analyze the concept
3. Create a minimal Mermaid diagram
4. Return ONLY the diagram code, no explanations

Remember: KISS - Keep It Simple Stupid!
```

---

## 输出样式（Output Styles）

### 什么是输出样式？

输出样式让你自定义 Claude 的回复格式、语气和结构。

### 创建输出样式

```bash
/output-style:new
```

**交互式配置：**
```
描述你想要的样式：
- 响应样式：简洁、详细、全面
- 语气：正式、随意、教育性
- 格式：项目符号、编号列表、YAML
- 重点：任务完成、学习、代码质量
```

### 示例：最小化项目符号样式

```markdown
<!-- ~/.claude/output-styles/minimal-bullets.md -->
---
name: minimal-bullets
description: Concise bullet-point responses
---

# Communication Style

- Use bullet points for all responses
- Keep each point under 20 words
- No fluff or filler words
- Focus on actionable information

# Response Format

- Main point
  - Sub-point 1
  - Sub-point 2
- Next main point
```

### 示例：YAML 结构化输出

```markdown
<!-- .claude/output-styles/yaml-concise.md -->
---
name: yaml-concise
description: All responses in YAML format
scope: project
---

# Output Format

All responses MUST be in valid YAML format:

```yaml
task: "<task description>"
status: "in_progress|completed|pending"
actions:
  - description: "<what was done>"
    result: "<outcome>"
next_steps:
  - "<step 1>"
  - "<step 2>"
```

No prose, only YAML.
```

### 高级：带自动化工作流的样式

```markdown
<!-- .claude/output-styles/retro-ascii-blog.md -->
---
name: retro-ascii-blog
description: HTML pages with retro ASCII art
---

# Style

Generate HTML pages with:
- ASCII art headers
- Retro terminal aesthetic
- Monospace fonts

## Workflow

After each response:
1. Save the HTML to `output.html`
2. Open the file in the browser automatically
```

### 作用域

- **用户级**：`~/.claude/output-styles/` （所有项目）
- **项目级**：`.claude/output-styles/` （当前项目，优先级更高）

### 使用输出样式

```bash
# 列出可用样式
/output-style

# 切换样式
/output-style minimal-bullets
```

---

## Hooks 自动化

### 什么是 Hooks？

Hooks 是在特定事件发生时自动执行的 shell 命令或子代理。

### 可用的 Hook 事件

- `pre-tool-use`：工具使用前
- `post-tool-use`：工具使用后
- `pre-start`：Claude 启动前
- `post-start`：Claude 启动后

### 配置 Hooks

在 `settings.json` 中：

```json
{
  "hooks": {
    "post-tool-use": [
      {
        "tool": "edit",
        "command": "prettier --write ${file}"
      }
    ],
    "pre-tool-use": [
      {
        "tool": "bash",
        "agent": "security-checker"
      }
    ]
  }
}
```

### 实用 Hook 示例

**自动格式化：**
```json
{
  "hooks": {
    "post-tool-use": [
      {
        "tool": "edit",
        "command": "eslint --fix ${file}"
      }
    ]
  }
}
```

**安全检查：**
```json
{
  "hooks": {
    "pre-tool-use": [
      {
        "tool": "bash",
        "agent": "security-reviewer"
      }
    ]
  }
}
```

---

## GitHub 集成

### 设置 GitHub Actions

**前提条件：**
```bash
# 安装 GitHub CLI
brew install gh

# 认证
gh auth login
```

**安装 Claude GitHub App：**
```bash
# 必须在 Git 仓库目录中运行
cd your-repo
/install-github-app
```

**安装流程：**
1. 在浏览器中授权 Claude GitHub App
2. 选择要安装的工作流（@Claude、代码审查等）
3. 选择认证方式（订阅 token）
4. 自动创建 PR 添加工作流文件
5. 合并 PR 完成设置

### 使用 @claude 修复 Issue

**在 GitHub Issue 或 PR 评论中：**
```
@claude 能帮我修复这个问题吗？
```

**Claude 会：**
1. 在评论中显示待办清单
2. 自动执行代码更改
3. 提交更改
4. 提供创建 PR 的链接

### 提供上下文的重要性

**错误示例：**
- 没有 `CLAUDE.md`
- Claude 缺乏项目上下文
- 可能产生破坏性更改

**正确示例：**
```bash
# 在本地先创建项目内存
cd your-repo
claude
/init
# 审查并提交 CLAUDE.md
git add CLAUDE.md
git commit -m "docs: add Claude context"
git push
```

现在 Claude 在 GitHub 上有完整的项目上下文！

---

## 高级工作流

### Plan Mode（规划模式）

```bash
# 进入规划模式（Shift + Tab）
```

**特点：**
- 只读模式
- 不修改文件
- 创建详细实施计划
- 可迭代改进

**工作流：**
1. **规划**：让 Claude 创建详细方案
2. **审查**：检查并提供反馈
3. **迭代**：优化计划
4. **批准**：确认后执行
5. **实施**：Claude 按计划修改代码

**示例：**
```
进入规划模式：
根据 @memory/spec/CLAUDE.md 创建实施计划：
- 数据结构
- UI 组件
- API 集成
```

### 并行 Claude 实例

**场景：** 同时运行多个 Claude Code 实例加速开发

**最佳实践：**

✅ **适合并行的任务：**
- 修复不相关的 bug
- 构建独立的 UI 页面
- 创建独立的组件

❌ **不适合并行的任务：**
- 有依赖关系的任务（前端 + 后端 API）
- 会产生合并冲突的任务
- 需要协调的任务

**示例：**
```bash
# 终端 1
cd hookhub
claude
# 任务：重新设计 HookCard 组件

# 终端 2
cd hookhub
claude
# 任务：重新设计主页 hero 区域
```

### Spec-Driven Development（规格驱动开发）

**流程：**

1. **创建规格文档**
```bash
# 使用规划模式
创建一个规格文件描述 HookHub 项目的 MVP
```

2. **保存为内存**
```
memory/spec/CLAUDE.md
```

3. **从规格实施**
```
根据 @memory/spec/CLAUDE.md 实现主页网格
```

**好处：**
- 清晰的需求定义
- 可审查的计划
- 团队对齐
- 可复用的上下文

### 专门化的 AI 环境

**目标：** 为不同任务创建独立的 Claude 实例

**方法 1：不同目录**
```bash
# 前端环境
mkdir ~/claude-frontend
cd ~/claude-frontend
claude
# 加载前端内存和样式

# 后端环境
mkdir ~/claude-backend
cd ~/claude-backend
claude
# 加载后端内存和样式
```

**方法 2：Git Worktrees**
```bash
git worktree add ../hookhub-feature feature-branch
cd ../hookhub-feature
claude
```

---

## 最佳实践

### 1. 上下文管理

**DO：**
- ✅ 使用 `/init` 为新项目创建基础上下文
- ✅ 保持 CLAUDE.md 简洁明确
- ✅ 使用多个专门的内存文件（`memory/frontend/`, `memory/backend/`）
- ✅ 定期使用 `/compact` 压缩长对话
- ✅ 用 `@` 明确引用需要的上下文

**DON'T：**
- ❌ 不要在 CLAUDE.md 中放太多信息
- ❌ 不要让对话无限增长
- ❌ 不要忽视上下文污染

### 2. 安全

**DO：**
- ✅ 审查 Claude 生成的所有代码
- ✅ 在子代理和自定义命令中使用 `allowed-tools`
- ✅ 仅在受信任的项目目录中运行 Claude
- ✅ 使用 API Key 时设置支出限制

**DON'T：**
- ❌ 不要盲目执行代码
- ❌ 不要从根目录运行 Claude
- ❌ 不要泄露 API Key
- ❌ 不要忽视提示注入风险

### 3. 模型选择

**使用 Sonnet 的场景：**
- 日常编码任务
- 实现已定义的功能
- 快速迭代
- 成本敏感的任务

**使用 Opus 的场景：**
- 复杂的架构设计
- 深度规划和研究
- 理解大型代码库
- 需要高级推理

### 4. 工作流优化

**单人开发：**
```
规划模式 → 创建规格 → 实施 → 测试 → 迭代
```

**团队开发：**
```
共享 CLAUDE.md → 统一编码规范 → 自定义命令 → Hooks 自动化
```

**大型重构：**
```
规划模式 → 详细方案 → 并行实例 → 分步实施 → 持续审查
```

### 5. 提示技巧

**明确性：**
```
❌ "改进这个函数"
✅ "重构 @src/utils/api.ts 中的 fetchData 函数，添加错误处理和重试逻辑"
```

**上下文：**
```
❌ "创建登录页面"
✅ "根据 @memory/spec/CLAUDE.md 中的设计规范，创建登录页面。使用 @memory/frontend/CLAUDE.md 中的组件库和样式指南"
```

**迭代：**
```
第一次：创建基础实现
第二次：添加错误处理
第三次：优化性能
第四次：添加测试
```

---

## 快捷键参考

| 快捷键 | 功能 |
|--------|------|
| `Tab` | 自动补全 |
| `Shift + Tab` | 进入规划模式 |
| `#` | 快速添加到内存 |
| `@` | 引用文件或内存 |
| `Ctrl + C` | 中断当前任务 |
| `/` | 显示斜杠命令列表 |

---

## 常见问题

### 1. Claude 超出上下文限制

**解决方案：**
```bash
/compact  # 压缩对话
# 或
/clear    # 清空重新开始（保留项目内存）
```

### 2. Claude 产生错误代码

**原因：** 缺乏上下文

**解决方案：**
```bash
/init  # 创建项目上下文
# 或手动创建详细的 CLAUDE.md
```

### 3. 需要使用特定库的最新文档

**解决方案：**
```bash
claude mcp add --scope project context7 https://mcp.context7.com/mcp
/exit && claude
# 然后在 CLAUDE.md 中添加规则自动使用
```

### 4. 想要不同的输出格式

**解决方案：**
```bash
/output-style:new
# 创建自定义输出样式
```

### 5. 需要自动化重复任务

**解决方案：**
- 创建自定义命令（`.claude/commands/`）
- 配置 Hooks（`settings.json`）
- 使用子代理处理特定任务

---

## 资源链接

- **官方文档**: https://docs.anthropic.com/claude/docs/claude-code
- **GitHub**: https://github.com/anthropics/claude-code
- **Cheatsheet**: https://awesomeclaude.ai/code-cheatsheet
- **社区**: https://discord.gg/anthropic

---

## 总结

Claude Code 不仅仅是一个 AI 编码助手，它是一个完整的开发环境增强系统。通过掌握：

1. **上下文工程** - 为 AI 提供正确的信息
2. **内存系统** - 持久化项目知识
3. **子代理** - 委托专门任务
4. **自定义命令** - 自动化工作流
5. **输出样式** - 定制 AI 沟通方式

你可以构建一个高度个性化、高效的 AI 辅助开发环境，显著提升开发效率和代码质量。

**核心原则：**
- 提供清晰的上下文
- 迭代优化
- 审查所有输出
- 自动化重复任务
- 持续学习和改进

祝你编码愉快！🚀

