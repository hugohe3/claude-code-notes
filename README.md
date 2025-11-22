# Claude Code 完整学习资源

欢迎！这里是 Claude Code 的完整学习和参考资料。

## 📚 文档结构

### 1. [完整使用指南](./Claude-Code-使用指南.md) 
**推荐新手从这里开始** ⭐

详细的教程和概念讲解，包括：
- 核心概念（Context Engineering）
- 安装与配置
- 所有功能详解
- 实用示例
- 最佳实践
- 常见问题

**适合：** 
- 初次接触 Claude Code
- 需要理解底层原理
- 想要系统学习

---

### 2. [快速参考卡片](./Claude-Code-快速参考.md)
**日常使用查询** 🚀

简洁的速查表，包括：
- 常用命令速查
- 配置模板
- 快捷键
- 故障排除
- 最佳实践核心

**适合：** 
- 已经熟悉基础
- 需要快速查找命令
- 作为桌面速查表

---

## 🎯 学习路径建议

### 第一天：基础入门
1. 阅读[快速开始](./Claude-Code-使用指南.md#快速入门)
2. 安装 Claude Code
3. 运行 `/init` 初始化第一个项目
4. 尝试基本对话

### 第二天：理解上下文
1. 学习[核心概念](./Claude-Code-使用指南.md#核心概念)
2. 理解 Context Engineering
3. 创建自己的 `CLAUDE.md`
4. 使用 `@` 和 `#` 管理上下文

### 第三天：自定义功能
1. 创建第一个[自定义命令](./Claude-Code-使用指南.md#自定义命令)
2. 尝试[输出样式](./Claude-Code-使用指南.md#输出样式output-styles)
3. 配置[MCP 服务器](./Claude-Code-使用指南.md#mcp-集成)

### 第四天：高级特性
1. 创建[子代理](./Claude-Code-使用指南.md#子代理subagents)
2. 设置[Hooks](./Claude-Code-使用指南.md#hooks-自动化)
3. 尝试[规划模式](./Claude-Code-使用指南.md#plan-mode规划模式)

### 第五天：团队协作
1. 配置[GitHub 集成](./Claude-Code-使用指南.md#github-集成)
2. 设置团队共享的配置
3. 创建项目规范文档

---

## 🔥 必知必会

### 核心命令（前 10）

```bash
/init              # 初始化项目上下文
/memory            # 编辑内存文件
/clear             # 清空对话
/compact           # 压缩对话
/agents            # 管理子代理
/mcp               # 管理 MCP 服务器
/output-style      # 切换输出样式
/config            # 打开配置
/cost              # 查看成本
Shift + Tab        # 进入规划模式
```

### 核心概念（前 5）

1. **Context Engineering** - 给 AI 正确的上下文
2. **内存系统** - 分层持久化知识
3. **子代理** - 专门任务的专门助手
4. **输出样式** - 自定义 AI 沟通方式
5. **Hooks** - 自动化工作流

---

## 💡 快速技巧

### 提升效率的 5 个技巧

1. **总是先 `/init`**
   ```bash
   cd new-project
   claude
   /init
   ```

2. **使用 `@` 明确引用**
   ```
   根据 @memory/spec/CLAUDE.md 实现功能
   ```

3. **定期压缩对话**
   ```bash
   /compact  # 当对话变长时
   ```

4. **创建专门的内存文件**
   ```
   memory/
   ├── frontend/CLAUDE.md
   ├── backend/CLAUDE.md
   └── database/CLAUDE.md
   ```

5. **使用规划模式处理复杂任务**
   ```
   Shift + Tab → 规划 → 审查 → 执行
   ```

---

## 📖 实用示例

### 示例 1：初始化新项目

```bash
# 创建 Next.js 项目
npx create-next-app@latest my-app
cd my-app

# 启动 Claude Code
claude

# 初始化项目上下文
/init

# 开始开发
帮我创建一个用户认证系统
```

### 示例 2：使用子代理进行代码审查

```bash
# 创建代码审查子代理
/agents
# 选择 Project → Generate with Claude
# 提示：创建一个严格的代码审查员

# 使用子代理
请审查 @src/auth/login.ts
```

### 示例 3：自动提交消息

创建 `.claude/commands/commit.md`：

```markdown
---
name: commit
description: Generate smart commit message
allowed-tools:
  - git status
  - git diff
  - git commit
---

# Instructions

!git status
!git diff --staged

Generate a Conventional Commits format message.
Context: $arguments
```

使用：
```bash
/commit 添加用户认证功能
```

### 示例 4：设置 MCP 获取最新文档

```bash
# 添加 Context7
claude mcp add --scope project \
  context7 https://mcp.context7.com/mcp

# 重启
/exit
claude

# 在 CLAUDE.md 中添加
每次询问 React 问题时使用 context7 MCP

# 测试
React 19 的最新特性是什么？
```

---

## 🎓 进阶学习

### 推荐阅读顺序

1. **基础** → [完整使用指南](./Claude-Code-使用指南.md)
2. **实践** → 按照学习路径动手操作
3. **参考** → 使用[快速参考](./Claude-Code-快速参考.md)日常查询
4. **深入** → 阅读官方文档高级特性

### 外部资源

- 📘 [官方文档](https://docs.anthropic.com/claude/docs/claude-code)
- 📊 [Cheatsheet](https://awesomeclaude.ai/code-cheatsheet)
- 💻 [GitHub 仓库](https://github.com/anthropics/claude-code)
- 💬 [Discord 社区](https://discord.gg/anthropic)

---

## 🆘 遇到问题？

### 常见问题快速解决

| 问题 | 快速解决 | 详细说明 |
|------|----------|----------|
| 生成错误代码 | 运行 `/init` | [详见](./Claude-Code-使用指南.md#2-claude-产生错误代码) |
| 上下文超限 | 运行 `/compact` | [详见](./Claude-Code-使用指南.md#1-claude-超出上下文限制) |
| 需要最新文档 | 添加 Context7 MCP | [详见](./Claude-Code-使用指南.md#3-需要使用特定库的最新文档) |
| 想要特定格式 | `/output-style:new` | [详见](./Claude-Code-使用指南.md#4-想要不同的输出格式) |
| 自动化任务 | 创建自定义命令 | [详见](./Claude-Code-使用指南.md#5-需要自动化重复任务) |

### 获取帮助

1. **查看文档** - 先查本地文档
2. **搜索 Issues** - GitHub 仓库
3. **询问社区** - Discord
4. **联系支持** - Anthropic 官方

---

## 🎯 目标达成检查清单

完成这些里程碑，你就掌握了 Claude Code！

### 初级 ✅
- [ ] 成功安装 Claude Code
- [ ] 完成第一次项目初始化（`/init`）
- [ ] 创建自己的第一个 `CLAUDE.md`
- [ ] 使用 `@` 引用文件
- [ ] 使用 `#` 快速添加内存

### 中级 ✅
- [ ] 创建自定义命令
- [ ] 配置 MCP 服务器
- [ ] 创建输出样式
- [ ] 使用规划模式完成任务
- [ ] 理解并使用 `/compact`

### 高级 ✅
- [ ] 创建并使用子代理
- [ ] 配置 Hooks 自动化
- [ ] 设置 GitHub 集成
- [ ] 运行并行 Claude 实例
- [ ] 实施规格驱动开发

### 专家 ✅
- [ ] 为团队定制完整配置
- [ ] 创建专门化的 AI 环境
- [ ] 构建复杂的多代理工作流
- [ ] 优化上下文工程策略
- [ ] 贡献社区最佳实践

---

## 🌟 成为 Claude Code 专家

### 关键原则

1. **Context is King** 🎯
   - 始终提供清晰的上下文
   - 使用分层的内存系统
   - 明确引用相关文件

2. **Iterate, Don't Perfect** 🔄
   - 先快速实现
   - 然后逐步优化
   - 每次专注一个改进

3. **Automate Repetition** ⚙️
   - 重复 3 次？创建命令
   - 每次都做？设置 Hook
   - 特定任务？建子代理

4. **Review Everything** 👀
   - AI 可能犯错
   - 始终审查输出
   - 理解每一行代码

5. **Share Knowledge** 🤝
   - 团队共享配置
   - 文档化最佳实践
   - 贡献社区

---

## 📝 快速备忘

### 最小化启动流程

```bash
# 1. 进入项目
cd project

# 2. 启动
claude

# 3. 初始化
/init

# 4. 开始编码！
```

### 黄金公式

```
清晰的上下文 + 明确的指令 + 迭代优化 = 高质量输出
```

### 记住

- **上下文** > 提示
- **简洁** > 冗长  
- **明确** > 模糊
- **审查** > 信任

---

## 🚀 开始你的 Claude Code 之旅！

选择你的路径：

- 🆕 **新手？** → 从[完整使用指南](./Claude-Code-使用指南.md)开始
- ⚡ **赶时间？** → 看[快速参考](./Claude-Code-快速参考.md)
- 🎯 **有目标？** → 查看上面的示例和检查清单

**记住：** 最好的学习方式是动手实践。现在就打开终端，输入 `claude`，开始你的 AI 辅助开发之旅吧！

---

*最后更新：2025-11-22*  
*基于 Claude Code 2.0*

祝编码愉快！🎉

