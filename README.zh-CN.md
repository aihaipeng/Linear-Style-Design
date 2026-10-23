# Linear Style Design

<p align="center"><a href="./README.md">English</a> · 简体中文</p>

一个 [Agent Skill](https://agentskills.io)，把 Linear 的设计语言——近中性表面、克制的排版、稳定的信息密度、快速可预期的交互——带到**任何前端技术栈**：Web、SSR、原生应用和设计工具。

## 你将获得

- **一份随手可查的默认值速查表**——颜色、字阶、间距、控件高度、圆角、动效时长、对比度底线——直接写在 [`SKILL.md`](linear-style-design/SKILL.md) 里
- **9 个聚焦的参考文件**，按需加载：色彩、排版、布局、组件、动效 + 无障碍、实现、微细节、审查清单、原则与证据
- **一张即拷即用的 CSS token 表**（[`assets/linear-tokens.css`](linear-style-design/assets/linear-tokens.css)），含亮/暗双主题，零构建
- **一套审查工作流**：分级的反模式表（Fail / Needs reason 两档）、渲染审查步骤与微细节扫查、五个审查问题、完成定义（DoD），以及带硬性封顶规则的结论报告格式——用于审计现有界面

## 安装

把 [`linear-style-design/`](linear-style-design) 文件夹复制进你 agent 的 skills 目录即可，如 Claude Code 的 `~/.claude/skills/`（个人级）或项目内 `.claude/skills/`（团队共享）；Claude.ai 则作为自定义 skill 上传。文件夹遵循开放的 [Agent Skills 规范](https://agentskills.io/specification)，GitHub Copilot、Cursor、Codex、Gemini CLI、OpenCode 等同样适用。

## 使用

| 意图 | 示例提示词 |
|---|---|
| 构建 | “用 React 做一个 Linear 风格的设置页，品牌色用我们自己的，不要靛蓝。” |
| 改造 | “让这个后台更接近 Linear 的感觉——保留我们的字体和数据。” |
| 审计 | “按 Linear 设计语言审查这个页面，列出真实存在的问题。” |

## 示例

用同一份提示词、不同模型生成的两个单文件 CI/CD 流水线监控台：

| 文件 | 模型 |
|---|---|
| [`examples/index.html`](examples/index.html) | glm-5.3-flash |
| [`examples/pipeline-monitor.html`](examples/pipeline-monitor.html) | deepseek-4.1-flash |

提示词：

```text
Generate a complete, runnable web page as a single HTML file that works by
simply opening it in a browser. No frameworks, no build tools.

Subject: A CI/CD pipeline monitor — an internal workbench where an engineering
team watches the build and deploy status of multiple services. Builds run,
queue, succeed, and fail; people need to scan the whole system at a glance,
locate problems, and take action.

Make full use of the linear-style-design skill. Everything else — naming,
structure, layout, colors, data, interactions — is your call.
```

## 仓库结构

```
├── LICENSE                  	# MIT
├── README.md                	# 英文说明
├── README.zh-CN.md          	# 中文说明
├── examples/
│   ├── index.html            	# 示例：流水线监控台（glm-5.3-flash）
│   └── pipeline-monitor.html 	# 示例：流水线监控台（deepseek-4.1-flash）
└── linear-style-design/     	# skill 本体——复制这个文件夹
    ├── SKILL.md             	# 入口：设计 DNA、默认值、路由
    ├── references/          	# 9 个主题文件，按需加载
    └── assets/
        └── linear-tokens.css
```

## 30 秒了解这套风格

| 维度 | 经验法则 |
|---|---|
| 注意力 | 内容先于应用外壳；导航永不发光 |
| 色彩 | 近中性基底 + 一个品牌强调色族；语义色与数据色分开 |
| 结构 | 先用分组和留白，再考虑分隔线；不是什么都装进卡片 |
| 密度 | 按输入方式和任务选择——紧凑桌面 ≠ 等比缩小的移动端 |
| 反馈 | 快速本地响应、状态明确、失败可恢复 |

默认值随上下文变化：**生产力工具 UI**（紧凑、键盘优先）、**营销页**（排版纪律、真实产品影像）、**文档 / 设置 / 移动阅读**（可读性优先）。参见 [`SKILL.md`](linear-style-design/SKILL.md) 中的 "Three context types"。

## 许可证

[MIT](./LICENSE)
