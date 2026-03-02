大家好，我是算法工程笔记。

最近 Agent 开发圈子里最火的资料，莫过于 Claude Code 的 Skills。
而在这背后，隐藏着一个关于Agent开发更大的宝藏库——Anthropic engineering 系列博客。
这个系列博客仅仅是技术文档，更是一线工程师在生产环境中“踩坑”后总结出来的**生存指南**。

由于干货实在太多，我们将其拆分为（上）（下）两篇。今天这**上篇**，主要聚焦于 **Agent 的核心模式与工具链**。

---

## 1. [Contextual Retrieval in AI Systems](https://www.anthropic.com/engineering/contextual-retrieval)

> “让每一个切片都能‘自报家门’。”

传统 RAG 系统最大的痛点是切分（Chunking）导致的**上下文丢失**。一段代码或一句话一旦脱离了前文，从语义上就变得“面目全非”，导致检索失败。

Anthropic 的解法非常直观：**Contextual Retrieval**。简单来说，就是在做 Embedding 和索引之前，先让模型给每个切片加一段“背景介绍”。配合 **Contextual BM25**，这套组合拳精准解决了“切得太碎找不到”的问题。

## 2. [Building Effective AI Agents](https://www.anthropic.com/engineering/building-effective-agents)

> “不要为了炫技而强行 Agent，有时候一个工作流就够了。”

这篇文章总结了构建 Agent 的核心模式。从简单的增强型 LLM，到 Prompt Chaining、Routing，再到复杂的 Orchestrator-workers 和 Evaluator-optimizer。

它的核心观点非常务实：**控制力与灵活性的平衡**。不要一上来就搞全自动 Agent，根据任务复杂度选择最简单的有效模式，才是工程落地的正道。

## 3. [The "think" tool: Enabling Claude to stop and think](https://www.anthropic.com/engineering/claude-think-tool)

> “赋予模型‘三思而后行’的权利。”

在处理复杂策略或多步推理时，模型往往容易“嘴比脑子快”，导致决策错误。

**think tool** 就是让 Claude 在行动之前，先输出一段结构化的思考过程。这不仅降低了错误率，更重要的是给了模型一个“打草稿”的空间，让决策逻辑更加连贯和可追溯。

## 4. [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)

> “像带实习生一样带 Agent。”

Agentic coding 工具虽好，但上手有门槛。这篇文章是一份实战避坑指南：如何配置环境、如何优化工具、为什么要用 TDD（测试驱动开发）。

特别提到的一点是**视觉反馈**的重要性——让 Agent 看到它改完代码后的效果，是提升成功率的关键。

## 5. [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)

> “不仅要分工，更要协作。”

单兵作战的 Agent 在面对需要广度搜索的复杂研究任务时，往往力不从心。Anthropic 分享了他们的 **Orchestrator-Worker** 架构：主 Agent 负责统筹和拆解，子 Agent 并行干活。这展示了如何用架构设计来突破单模型的上下文和能力瓶颈。

## 6. [Writing effective tools for AI agents—using AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents)

> “用魔法打败魔法，用 Agent 写 Agent 工具。”

传统软件开发那种“确定性”的思维，在 Agent 交互中往往行不通。这篇文章提供了一个极其巧妙的思路：利用 Agent 自身来构建原型、生成评估任务、分析结果，最终优化工具定义。让 Agent 告诉你，它觉得什么样的工具最好用。

---

由于篇幅原因，关于 **SDK、上下文工程、Agent Skills** 以及 **MCP** 等更硬核的工程基建内容，我们将在（下）篇中详细解读。

欢迎关注“算法工程笔记”，不错过下篇更新。

<!-- 
Prompt for Cover Image:
A minimalist, rational style tech illustration for a blog cover, aspect ratio 16:9. 
Visualizing Part 1: Core Patterns & Tools. 
Central motif: A set of glowing, golden modular blocks (representing core tools) being carefully assembled by invisible hands. 
Aesthetic: 'Algorithm Engineering' vibe - professional, flat vector art, clean lines, soft tech blue and cool grey color palette, white background, no text. Rational, calm, and sophisticated.
-->
