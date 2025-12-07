# Claude Code 快速参考卡片

## ⚡ 快速开始

```bash
# 安装
brew install claude-code

# 启动
cd your-project
claude

# 初始化项目
/init
```

---

## 📋 常用斜杠命令

| 命令 | 功能 |
|------|------|
| `/init` | 分析项目并创建 CLAUDE.md |
| `/clear` | 清空对话历史 |
| `/compact` | 压缩对话历史 |
| `/memory` | 编辑内存文件 |
| `/agents` | 管理子代理 |
| `/mcp` | 管理 MCP 服务器 |
| `/config` | 打开配置面板 |
| `/cost` | 查看成本 |
| `/output-style` | 管理输出样式 |
| `/hooks` | 管理自动化钩子 |
| `/ide` | 安装 IDE 扩展 |
| `/install-github-app` | 安装 GitHub 集成 |

---

## 🧠 内存系统速查

### 内存层级

```
~/.claude/CLAUDE.md              # 全局用户内存
└── project/CLAUDE.md            # 项目内存
    └── project/memory/          # 专门上下文
        ├── frontend/CLAUDE.md
        ├── backend/CLAUDE.md
        └── spec/CLAUDE.md
```

### 快捷操作

```bash
/memory                    # 编辑内存
# 输入文本快速添加内存     # 使用 # 快捷键
@memory/spec/CLAUDE.md    # 引用特定内存
```

### 内存模板

```markdown
# 项目概述
[简短描述]

## 技术栈
- 框架/语言
- 主要库

## 编码规范
- 规则 1
- 规则 2

## 架构
- 目录结构说明

## 常用命令
- 开发命令
- 构建命令
```

---

## 🔧 自定义命令速查

### 基础命令

```markdown
<!-- .claude/commands/your-command.md -->
---
name: your-command
description: What it does
---

# Instructions

Your instructions here.

Use $arguments for user input.
```

### 高级命令（带 Bash）

```markdown
---
name: smart-commit
description: Smart commit with context
allowed-tools:
  - git status
  - git diff
  - git commit
---

# Instructions

!git status
!git diff --staged

Generate commit message based on changes.
User hint: $arguments
```

---

## 🤖 子代理速查

### 创建子代理

```bash
/agents
# 选择 Project → Generate with Claude
```

### 手动配置

```markdown
<!-- .claude/agents/reviewer.md -->
---
name: Code Reviewer
description: Reviews code for issues
model: claude-sonnet-4-latest
color: yellow
allowed-tools:
  - read
  - grep
---

# System Prompt

You are an expert code reviewer...

[详细指令]
```

### 调用子代理

```
请对 @main.py 进行代码审查
# 根据 description 自动触发
```

---

## 🎨 输出样式速查

### 创建样式

```bash
/output-style:new
```

### 样式模板

```markdown
<!-- .claude/output-styles/style-name.md -->
---
name: style-name
description: Description
---

# Communication Style
- 规则 1
- 规则 2

# Response Format
[格式要求]

# Tone Guidelines
[语气指导]
```

### 常用样式示例

**简洁项目符号：**
```markdown
- 使用项目符号
- 保持简洁
- 不超过 20 字/点
```

**YAML 结构化：**
```markdown
所有回复必须是有效的 YAML：
```yaml
task: "任务描述"
status: "状态"
actions:
  - "操作 1"
```

---

## 🔌 MCP 集成速查

### 添加 MCP

```bash
# 项目级
claude mcp add --scope project \
  context7 https://mcp.context7.com/mcp

# 用户级
claude mcp add --scope user \
  context7 https://mcp.context7.com/mcp

# 重启生效
/exit
claude
```

### 使用 MCP

**显式使用：**
```
使用 context7 MCP 查找 React 最新文档
```

**自动使用（在 CLAUDE.md 中）：**
```markdown
每次询问 React 时自动使用 context7 MCP
```

---

## 🪝 Hooks 速查

### 配置文件位置

```
.claude/settings.json
~/.claude/settings.json
```

### Hook 示例

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

### 可用事件

- `pre-tool-use` - 工具使用前
- `post-tool-use` - 工具使用后
- `pre-start` - 启动前
- `post-start` - 启动后

---

## 🐙 GitHub 集成速查

### 设置

```bash
# 1. 安装 GitHub CLI
brew install gh

# 2. 认证
gh auth login

# 3. 在 Git 仓库中安装
cd your-repo
/install-github-app
```

### 使用

**在 Issue/PR 评论中：**
```
@claude 修复这个 bug
```

**重要：** 先创建 CLAUDE.md 提供上下文！

```bash
claude
/init
git add CLAUDE.md
git commit -m "docs: add context"
git push
```

---

## 🚀 工作流速查

### 规划模式

```
Shift + Tab 进入规划模式
→ 创建计划
→ 审查迭代
→ 批准执行
```

### 并行开发

```bash
# 终端 1
claude
# 任务 A（独立）

# 终端 2
claude
# 任务 B（独立）
```

**✅ 适合：** 独立的 bug、独立的页面、独立的组件

**❌ 不适合：** 有依赖的任务（前端+后端）

### 规格驱动开发

```
1. 规划模式 → 创建详细规格
2. 保存到 memory/spec/CLAUDE.md
3. 引用规格实施：根据 @memory/spec/CLAUDE.md 实现
```

---

## 💡 提示技巧

### 明确性

```
❌ "改进代码"
✅ "重构 @src/api.ts 中的 fetchData，添加错误处理和重试"
```

### 上下文

```
❌ "创建登录页"
✅ "根据 @memory/spec/CLAUDE.md 创建登录页，使用 @memory/frontend/CLAUDE.md 的组件库"
```

### 迭代

```
第1轮：基础实现
第2轮：错误处理
第3轮：性能优化
第4轮：测试
```

---

## ⚙️ 配置层级

```
用户配置: ~/.claude/settings.json
  └─ 项目配置: .claude/settings.json
      └─ 本地配置: .claude/settings.local.json
```

**优先级：** 本地 > 项目 > 用户

---

## 🎯 最佳实践核心

### 上下文管理
- ✅ 使用 `/init` 创建基础上下文
- ✅ 保持 CLAUDE.md 简洁
- ✅ 使用专门的内存目录
- ✅ 定期 `/compact` 压缩对话

### 安全
- ✅ 审查所有生成的代码
- ✅ 使用 `allowed-tools` 限制权限
- ✅ 仅在受信任目录运行
- ✅ 设置 API 支出限制

### 模型选择
- **Sonnet**：日常编码、快速迭代
- **Opus**：复杂设计、深度规划

---

## 🔑 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Tab` | 自动补全 |
| `Shift + Tab` | 规划模式 |
| `#` | 快速添加内存 |
| `@` | 引用文件/内存 |
| `Ctrl + C` | 中断任务 |
| `/` | 命令列表 |

---

## 🎯 Agent Skills 速查

### Skills vs 其他原语

| 原语 | 用途 | 上下文消耗 |
|------|------|-----------|
| **Skills** | 程序化知识 | 极低（渐进式加载） |
| **MCP** | 外部资源 | 较高（预先定义） |
| **Subagents** | 隔离任务 | 独立窗口 |

### 创建 Skill

```
.claude/skills/
└── my-skill/
    ├── SKILL.md         # 定义文件
    └── helper.sh        # 辅助脚本
```

**SKILL.md 模板：**
```markdown
---
name: Skill Name
description: What it does
---

# Instructions
[详细指令]

!bash .claude/skills/my-skill/helper.sh
```

### 何时使用

- **Skills**：一致方法论、低上下文开销
- **MCP**：外部 API、数据库访问
- **Subagents**：长周期任务、需要隔离

---

## 🖥️ Desktop 模式速查

### 两种模式

| 模式 | 特点 | 适用场景 |
|------|------|----------|
| **Local Worktree** | 本地执行，Git Worktree | 日常开发 |
| **Default (Cloud)** | 云端容器，GitHub 集成 | 并行任务、无限扩展 |

### Git Worktrees

```bash
# 创建 worktree
git worktree add ../feature-dir feature-branch

# 列出 worktrees
git worktree list

# 删除 worktree
git worktree remove ../feature-dir
```

### 多代理协调

```
任务 A: Claude Chat → 研究
任务 B: Local Mode → 功能开发
任务 C: Cloud Mode → 远程开发
           ↓
    合并所有工作
```

### Mobile 流程

```
📱 移动端发起任务
    ↓ (云容器执行)
创建 PR / 推送代码
    ↓
💻 桌面端验证和修复
```

---

## 🆘 故障排除

| 问题 | 解决方案 |
|------|----------|
| 上下文超限 | `/compact` 或 `/clear` |
| 生成错误代码 | 创建/改进 CLAUDE.md |
| 需要最新文档 | 添加 Context7 MCP |
| 想要特定格式 | `/output-style:new` |
| 重复性任务 | 创建自定义命令或 Hooks |

---

## 📚 资源

- **文档**: https://docs.anthropic.com/claude/docs/claude-code
- **Cheatsheet**: https://awesomeclaude.ai/code-cheatsheet
- **GitHub**: https://github.com/anthropics/claude-code

---

## 💰 定价参考

| 计划 | 价格 | 特点 |
|------|------|------|
| Pro | $17-20/月 | Sonnet 模型 |
| Max | $100+/月 | Opus + Sonnet，5x 用量 |
| API Key | 按用量 | 灵活，需设置限额 |

**推荐：** 订阅制（Pro/Max）更可预测

---

**记住：** Claude Code 的核心是 **Context Engineering** - 给 AI 正确的上下文，就能得到正确的输出！🎯

---

*最后更新：2025-12-07*

