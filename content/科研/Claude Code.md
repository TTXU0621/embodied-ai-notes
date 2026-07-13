---
tags:
  - claude-code
  - vibe-coding
  - easy-vibe
  - agentic-engineering
source: "https://datawhalechina.github.io/easy-vibe/zh-cn/stage-3/core-skills/basics/"
updated: 2026-06-19
---

# Claude Code 快速上手核心指南

> 来源：DataWhale Easy-Vibe 教程 — Stage 3 高级开发 > 核心技能 > Claude Code 深入浅出

---

## 一、Claude Code 基础

### 1.1 什么是 CLI AI 编程工具

Claude Code 是 Anthropic 推出的**终端 CLI AI 编程工具**，与 Cursor 等 IDE 内置 Agent 有本质区别：

| 特性 | Claude Code | Cursor |
|------|------------|--------|
| 自动任务执行 | ✅ 非常强 | ❌ 能力有限 |
| 多文件操作 | ✅ 非常强 | ⚠️ 还不错 |
| GitHub 一体化 | ✅ 可直接提交 | ⚠️ 需手动 |
| 上下文长度 | ✅ 极长 | ⚠️ 较好 |
| 调试辅助 | ✅ 自动化 | ⚠️ 较多手动 |
| IDE 集成 | ❌ 仅命令行 | ✅ 原生 VS Code |

### 1.2 安装与启动

```bash
# 安装（需 Node.js 18+）
npm install -g @anthropic-ai/claude-code

# 验证安装
claude --version

# 进入项目启动
cd your-project
claude
```

> 💡 也可以让 AI Agent 帮你安装：直接说「帮我装 anthropic 的 claude code」

首次启动需完成：登录 Anthropic 账户 → 选择使用计划 → 同意使用条款 → （可选）配置 API 密钥。

### 1.3 常用命令速查

| 命令 | 作用 |
|------|------|
| `claude` | 启动交互模式 |
| `claude "query"` | 执行一次性任务 |
| `claude -p "query"` | 执行问题并自动退出 |
| `claude -c` | 继续最近会话 |
| `/resume` | 切换回上一段会话 |
| `/init` | 用 CLAUDE.md 初始化项目说明 |
| `/clear` | 清空当前上下文 |
| `/compact` | 压缩会话历史 |

## 二、核心技巧（11 个）

### 技巧 1：双击 Esc 回退对话 — 撤销误操作

| 操作 | 效果 |
|------|------|
| 按一次 Esc | 清除当前正在输入的内容（类似 Ctrl+C） |
| 按两次 Esc | 回退到上一次对话状态（撤销上一轮对话） |
| 按三次 Esc | 清除所有对话历史（重新开始） |

> ⚠️ 注意：双击 Esc 回退的是**对话状态**，不是代码修改。文件修改需用 `git checkout` 手动恢复。

### 技巧 2：@ 引用文件 — 精准指定上下文

显式引用文件能让 AI 更准确理解意图，避免读取无关文件浪费 Token：

```
@src/utils.ts 解释这个文件
@src/app.tsx @src/components/Header.tsx 这两个文件的关系是什么？
@src/components/ 总结一下这个目录下的所有组件
@src/utils.ts:45-60 解释这段代码的作用
```

> 💡 输入 `@` 后按 Tab 键可自动补全文件路径。

### 技巧 3：! 执行命令 — 终端集成

无需切换终端窗口即可运行命令：

```
!npm test           # 运行测试
!git status         # 查看 Git 状态
!git diff           # 查看代码差异
!npm run build      # 构建项目
```

典型工作流：`!npm test` → 测试失败 → 「分析一下测试失败的原因，并修复代码」

### 技巧 4：/plan 先规划后编码 — 复杂任务的正确打开方式

对于超过 30 分钟的任务，先用 `/plan` 制定实施计划：

```
/plan
我想添加用户认证功能，请帮我制定实施计划
```

Claude 会：分析需求 → 评估现状 → 制定分阶段计划 → 与你确认后逐步执行。

### 技巧 5：/init 自动生成配置 — 快速初始化项目

`/init` 自动扫描项目，生成 `CLAUDE.md` 配置文件。这是 Claude Code 的"项目记忆"，包含技术栈、常用命令、代码规范等。

```
/init
```

> 新项目初始化后，立即运行 `/init`，然后根据实际情况调整生成的配置。

### 技巧 6：/compact 压缩上下文 — 节省 Token

长对话会消耗大量 Token。每 5-6 轮对话后使用 `/compact`，它会提取关键信息生成摘要，后续基于摘要继续工作。

```
# 长对话后
/compact
# 继续工作
现在我们来实现订单模块...
```

### 技巧 7：用 Claude Code 辅助 Git 提交

推荐工作流：先让 Claude 查看 diff、整理提交信息，再由你执行标准 Git 命令：

```bash
/diff                          # 1. 查看变更
!git status                    # 2. 查看状态
# 3. 让 Claude 生成提交信息
请基于当前 git diff，按照 Conventional Commits 规范生成一个 commit message
!git add -A                    # 4. 暂存
!git commit -m "feat: ..."     # 5. 提交
```

> 也可通过插件补回一键提交体验：安装 `commit-commands` 插件后使用 `/commit-commands:commit`

### 技巧 8：Shift+Tab 自动接受 — 提高流畅度

| 模式 | 行为 | 适用场景 |
|------|------|----------|
| 默认模式 | 每次修改都询问确认 | 学习阶段、重要代码 |
| 自动接受 | 直接应用修改 | 熟悉后、快速迭代 |

按 `Shift+Tab` 切换模式。建议配合 Git 使用，出问题时方便回滚。

### 技巧 9：Ctrl+C 取消操作 — 紧急制动

| 操作 | 效果 |
|------|------|
| 按一次 Ctrl+C | 取消当前正在执行的操作 |
| 按两次 Ctrl+C | 完全退出 Claude Code |

与双击 Esc 的区别：Ctrl+C 停止**操作**，Esc 回退**对话状态**。

### 技巧 10：/context 查看上下文使用 — 优化 Token 消耗

```
/context
```

显示 Token 使用率、文件引用数、最消耗 Token 的文件等。当使用率超过 70% 时考虑 `/compact`。

### 技巧 11：/resume 恢复会话 — 切换多任务对话

在处理多个任务时，`/resume` 能切换回之前的会话，保留完整上下文：

```
# 任务 1：修复 bug
claude> 修复登录页面的验证问题

# 任务 2：添加新功能（新会话）
claude> 添加用户注册功能

# 切换回任务 1
claude> /resume
```

---

## 三、核心配置

### 3.1 配置文件位置与优先级

Claude Code 采用分层配置策略（优先级从高到低）：

| 位置 | 作用域 | 用途 | 是否提交 Git |
|------|--------|------|--------------|
| `.claude/settings.local.json` | 项目本地 | 个人偏好设置 | ❌ 否 |
| `.claude/settings.json` | 项目共享 | 团队统一配置 | ✅ 是 |
| `~/.claude/settings.json` | 全局 | 个人默认配置 | ❌ 否 |

### 3.2 CLAUDE.md — 项目记忆

`CLAUDE.md` 是最重要的配置文件，相当于项目的"说明书"。每次启动时 Claude 自动读取。

**最小可用模板：**

```markdown
# [项目名称]

## 技术栈
- 框架：React 18 + TypeScript
- 状态管理：Zustand
- 样式方案：Tailwind CSS
- 构建工具：Vite

## 常用命令
npm run dev      # 启动开发服务器
npm run test     # 运行单元测试
npm run build    # 生产构建

## 代码规范
- 组件使用函数组件 + Hooks
- 文件命名：PascalCase（组件）、camelCase（工具函数）
- Git 提交使用 Conventional Commits 规范
```

> 运行 `/init` 可让 Claude 自动生成，生成后建议人工检查调整。

### 3.3 .claudeignore — 节省 Token

告诉 Claude 哪些文件不应被读取。合理配置能减少 40-60% Token 消耗。

**推荐配置：**

```
# 依赖目录
node_modules/

# 构建产物
dist/
build/
.next/

# 日志
*.log

# 环境变量（敏感信息）
.env
.env.local

# 大型资源文件
*.png
*.jpg
*.mp4
```

> 使用 `/context` 查看哪些文件消耗最多 Token，考虑加入忽略列表。

### 3.4 权限配置

在 `settings.json` 中精细控制操作权限（allow / ask / deny）：

```json
{
  "permissions": {
    "allow": [
      "Bash(git status)",
      "Bash(npm test:*)",
      "Edit(src/**/*.{ts,tsx})"
    ],
    "ask": [
      "Bash(git commit:*)",
      "Bash(git push:*)",
      "Bash(npm install:*)",
      "Read(.env)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(sudo:*)",
      "Edit(.git/*)"
    ]
  }
}
```

**渐进式策略：** 学习阶段保持默认 → 熟悉阶段将安全操作加入 allow → 高效阶段精细配置。

### 3.5 Rules 规则目录（大型项目）

大型项目可使用 Rules 目录模块化管理规范：

```
.claude/
├── settings.json
├── CLAUDE.md
└── rules/
    ├── 00-security.md       # 安全规则
    ├── 01-coding-style.md   # 编码风格
    ├── 10-api.md            # API 开发规范
    ├── 11-frontend.md       # 前端开发规范
    └── 20-testing.md        # 测试规范
```

规则文件支持 YAML frontmatter 控制适用范围（globs + commands + priority）。

---

## 四、核心操作指令

### 4.1 Slash 命令速查

| 命令 | 功能 | 使用场景 |
|------|------|----------|
| `/help` | 显示所有命令 | 忘记命令时快速查看 |
| `/init` | 初始化项目，生成 CLAUDE.md | 新项目或添加配置 |
| `/plan` | 进入规划模式 | 复杂任务前先制定计划 |
| `/clear` | 清除对话历史 | 上下文混乱时重新开始 |
| `/compact` | 压缩上下文 | 长对话后节省 Token |
| `/diff` | 打开交互式 diff 视图 | 查看当前未提交改动 |
| `/plugin` | 管理插件 | 安装提交、审查等扩展能力 |
| `/context` | 查看上下文使用 | 优化 Token 消耗 |
| `/cost` | 查看本次会话费用 | 关注使用成本 |
| `/config` | 打开配置面板 | 修改设置 |
| `/permissions` | 权限管理 | 调整操作权限 |

### 4.2 符号系统

| 符号 | 名称 | 用途 | 示例 |
|------|------|------|------|
| `/` | Slash 命令 | 执行内置操作 | `/help`, `/plan` |
| `@` | At 引用 | 引用文件/目录 | `@src/app.tsx` |
| `!` | Bang 模式 | 执行终端命令 | `!npm test` |
| `&` | 后台运行 | 后台执行任务 | `&npm run dev` |

### 4.3 完整工作流示例

**Bug 修复工作流：**

```bash
!npm test                           # 1. 运行测试
# 测试报错了，分析一下              # 2. 定位问题
@src/utils/validation.ts            # 3. 查看问题代码
# 修复 isEmail 函数                 # 4. 修复
!npm test                           # 5. 验证
# 请根据当前 diff 生成 commit msg   # 6. 提交
!git add -A && !git commit -m "..."
```

**新功能开发工作流：**

```bash
/plan                               # 1. 制定计划（我要添加购物车功能）
!git checkout -b feature/cart       # 2. 创建分支
# 按照计划逐步实现                  # 3. 开发
# 为购物车模块生成测试              # 4. 测试
!npm test                           # 5. 运行测试
/diff                               # 6. 审查变更
# 请生成 commit message             # 7. 提交
!git add -A && !git commit -m "..."
!git push                           # 8. 推送
```

---

## 五、Skills 完全指南

### 5.1 Skills 核心概念

Skills = 将专业知识、工作流程、最佳实践打包成**可复用技能包**，就像给 Claude 配备的"技能书"。

| 普通提示词 | Skills |
|-----------|--------|
| 临时性，每次重复 | 持久化，一次配置反复用 |
| 占用对话 Token | 按需加载，节省 Token |
| 无法共享 | 可团队共享、Git 版本控制 |

**三层渐进式加载架构：**

| 层级 | 内容 | 加载时机 | Token 消耗 |
|------|------|----------|------------|
| Layer 1: 元数据 | YAML frontmatter | Claude 启动时 | ~30-50 tokens/skill |
| Layer 2: 指令 | 完整 SKILL.md 内容 | Skill 被触发时 | ~5,000 tokens |
| Layer 3: 资源 | 脚本、模板、参考文档 | 按需通过文件系统访问 | 不占上下文 |

> 效果：100 个 Skill 启动仅消耗约 3-5K Token，比全量加载节省 **80%+ Token**。

### 5.2 快速上手三步走

**① 安装 find-skills（必装神器）：**
```bash
npx skills add vercel-labs/skills@find-skills -g -y
```

**② 一句话搜索安装 Skill：**
```
帮我找找 React 性能优化相关的技能
```

**③ 实战三大热门 Skill：**

| Skill | 用途 | 安装命令 |
|-------|------|---------|
| `remotion-dev/skills` | 用代码制作视频动画 | `npx skills add remotion-dev/skills -g` |
| `anthropics/skills/frontend-design` | 前端美化，去"AI 味" | `npx skills add anthropics/skills/frontend-design -g` |
| `frontend-slides` | 自然语言快速生成精美 PPT | 利用 find-skills 搜索安装 |

### 5.3 创建自定义 Skill

**方法一：直接对话创建**
```
请帮我创建一个名为 "format-code" 的 skill，功能是自动格式化代码。
要求：自动检测语言类型、应用格式化规则、返回格式化前后 diff
```

**方法二：使用官方 skill-creator**
```bash
npx skills add anthropics/skills@skill-creator -g
/skill-creator  # 按提示填写即可
```

### 5.4 SKILL.md 文件结构

```
my-skill/
├── SKILL.md          # 必需：核心定义文件
├── scripts/          # 可选：辅助脚本
├── templates/        # 可选：输出模板
├── references/       # 可选：参考文档
└── examples/         # 可选：示例文件
```

```markdown
---
name: my-skill
description: 技能描述，用于自动匹配触发
category: development
tags: [code, automation]
---

# 技能标题

## 使用场景

## 执行步骤

## 注意事项
```

### 5.5 Skills vs MCP 核心区别

| 维度 | Skills | MCP |
|------|--------|-----|
| **本质** | 知识和流程 | 工具和接口 |
| **提供什么** | 告诉 AI **"怎么做"** | 给 AI **"能用什么"** |
| **配置方式** | Markdown 文件 | JSON 配置文件 |
| **触发方式** | 自动匹配或命令调用 | 服务启动自动加载 |

> 💡 形象比喻：MCP 是给厨师的厨具（工具），Skills 是给厨师的菜谱（怎么做）。**两者互补，配合使用效果最佳。**

> 🔑 **核心原则**：如果你发现自己重复两次同样的指令，就应该创建一个 Skill！

---

## 六、常见问题

### 6.1 Token 消耗太快？

**诊断：** 先运行 `/context` 查看使用情况，关注 Token 使用率（>70% 需处理）和消耗最多 Token 的文件。

**优化策略：**

| 策略 | 操作 |
|------|------|
| 完善 .claudeignore | 忽略 node_modules、构建产物、日志、图片等 |
| 定期压缩上下文 | 每 5-6 轮对话后 `/compact` |
| 精准引用文件 | 用 `@src/utils/auth.ts` 而非 `@src/` |
| 避免读取大文件 | 考虑拆分成小模块 |

### 6.2 Claude 不理解项目？

1. 运行 `/init` 生成 `CLAUDE.md`
2. 手动补充架构决策、常见陷阱、第三方集成等
3. 大型项目使用 Rules 目录组织规范
4. 在指令中即时补充背景：「我们使用自定义的 useAuth Hook...」

### 6.3 如何回退操作？

| 场景 | 操作 |
|------|------|
| 回退对话 | 双击 Esc（回退对话状态，不撤销文件） |
| 撤销文件修改 | `git checkout -- filename.ts` |
| 撤销所有修改 | `git checkout -- .` |
| 撤销最近提交 | `git reset --soft HEAD~1`（保留变更）/ `--hard`（丢弃变更） |

> 🛡️ 最佳实践：使用 Claude Code 前先 `git commit` 或 `git stash` 保存当前状态。

### 6.4 权限提示太多？

在 `.claude/settings.json` 中配置权限：
- **allow**: 安全的常用操作（git status、npm test、编辑 src 目录）
- **ask**: 需要确认的操作（git commit/push、npm install、读 .env）
- **deny**: 危险操作（rm -rf、sudo、修改 .git 目录）

### 6.5 国内如何使用？

```bash
# 方案 1：使用 API 代理服务
export ANTHROPIC_BASE_URL="https://your-api-proxy.com/v1"
export ANTHROPIC_API_KEY="your-api-key"

# 方案 2：使用兼容 API 的国内大模型（见 1.5 节）

# 方案 3：让 AI Agent 帮你配置
# 「我要使用 Claude Code，API 地址是 xxx，密钥是 xxx，请帮我配置好环境变量」
```

---

## 七、Stage 3 完整技能树概览

```
Claude Code 精通层
├── CLAUDE.md 项目配置
├── Hooks 机制（PreToolUse / PostToolUse）
├── 自定义 Skills 开发
└── CI/CD 流水线集成

MCP 生态层
├── 连接飞书 / Notion / 数据库
├── 自定义 MCP Server 开发
└── 工作流中智能调度 MCP 工具

Agent 团队层
├── 专家角色设计（代码审查/测试/安全审计）
├── 任务分解与并行执行
└── 结果聚合与质量控制

跨平台交付层
├── PWA / Electron / 微信小程序
└── VSCode 扩展
```

---

## 八、社区资源推荐

| 仓库 | 说明 |
|------|------|
| `shanraisshan/claude-code-best-practice` | Boris Cherny（CC 负责人）维护，⭐57k+ |
| `jeffallan/claude-skills` | 66 个专业技能 + 300+ 参考文档 |
| `JackyST0/awesome-agent-skills` | 精选 Skills 资源列表 |
| `affaan-m/everything-claude-code` | Claude Code 综合资源合集 |

---

## 🔗 相关笔记

- [[vibe coding]]
- [[easy-vibe]]
- [[GitHub Vibe Coding教程精选]]
- [[ai agent]]
