---
author: AK27
title: "Anthropic 工程博客：Agent 开发者的生产环境生存指南（下）"
date: 2026-01-09
draft: false
tags: ["AI", "Agent", "Anthropic", "LLM", "工具链"]
categories: ["技术"]
ShowToc: true
---

大家好，我是算法工程笔记。

在上一篇中，我们探讨了 Agent 的核心模式、Think Tool 以及多 Agent 协作架构。今天这**下篇**，我们将深入到更底层的工程基建，聊聊 **SDK、上下文工程、MCP** 以及如何构建长期运行的 Agent 系统。

书接上回，让我们继续 Agent 的进阶之旅。

---

## 7. [Building agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)

> “给 Agent 装上‘手’和‘脚’。”

这是一篇工程实操指南。Claude Agent SDK 赋予了 Agent 操作文件系统、终端的能力，并支持 **Agentic search** 和 **Subagents**。它是构建通用、多步骤工作流的基石，解决了“虽然脑子好使，但是手够不着”的工程难题。

## 8. [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

> “注意力是稀缺资源，请省着点用。”

随着上下文窗口越来越大，大家都变得“大手大脚”。但这篇文章提出了 **Context Engineering** 的概念：通过精选示例、Just-in-time 检索和结构化笔记，把有限的注意力预算用在刀刃上，确保长任务中 Agent 不会“走神”。

## 9. [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

> “渐进式加载，让通用模型变身领域专家。”

通用模型缺乏领域知识，但全塞进 Prompt 又太贵太慢。**Agent Skills** 机制通过标准化的 `SKILL.md` 和代码组织，利用 **Progressive Disclosure**（渐进式披露）原则，按需加载专业能力。

## 10. [Making Claude Code more secure and autonomous with sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing)

> “安全感不是靠每一次弹窗换来的。”

在自主性与安全性之间，目前的解法往往是烦人的“是否允许执行”。Anthropic 引入了**沙盒机制**（文件与网络隔离），在保障底线安全的前提下，大幅减少了权限确认请求，解决了“点确认点到手软”的用户体验灾难。

## 11. [Code execution with MCP: building more efficient AI agents](https://www.anthropic.com/engineering/code-execution-with-mcp)

> “让 Agent 写代码去调用工具，而不是直接调用。”

对于大规模工具使用，直接通过 LLM 调度会导致上下文极度膨胀。这里提出了一个反直觉但高效的思路：结合 **MCP**，让 Agent 编写 Python 代码来批量调用工具和处理数据。不仅省 Token，还能处理复杂的中间逻辑。

## 12. [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

> “给 Agent 一个‘存档点’。”

针对跨上下文窗口的长任务，如何保证状态不丢？文章介绍了一种分离 Initializer 和 Coding Agent 的 Harness 设计，利用 `claude-progress.txt` 和 Git 历史作为“外挂显存”，确保 Agent 即使重启也能“接关”。

---

这两篇文章梳理了 Anthropic 在 Agent 工程化上的宝贵经验。从上篇的**模式与设计**，到下篇的**基建与优化**，这就是一份完整的 Agent 开发者生存指南。

建议大家结合自家业务场景尝试落地，哪怕只是借鉴其中一个点（比如 Contextual Retrieval 或 Think Tool），都可能带来质的提升。

感谢阅读，如果这篇内容对你有启发，欢迎点赞、转发和关注支持。

<!-- 
Prompt for Cover Image:
A minimalist, rational style tech illustration for a blog cover, aspect ratio 16:9. 
Visualizing Part 2: Engineering Infrastructure. 
Central motif: A robust, sleek blue infrastructure base-layer (representing SDK, Context, MCP) supporting the modular blocks from Part 1. 
Aesthetic: 'Algorithm Engineering' vibe - professional, flat vector art, clean lines, soft tech blue and cool grey color palette, white background, no text. Rational, calm, and sophisticated.
-->
