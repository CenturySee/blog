
大家好，我是算法工程笔记。

最近，从 Anthropic 的 Claude，到代码编辑器 Cursor，再到 Google 的 Antigravity，大家都在频繁提起一个词——**Skills**。

很多同学在后台问，这又是 AI 圈新造的“概念泡沫”吗？它和我们用了很久的 Tools（工具）到底有啥区别？

今天这篇，我们就来剥开概念的外壳，聊聊 **Skills 到底解决了 Agent 开发中的什么痛点，以及为什么它是 Agent 走向“工程化”的必经之路。**

---

## 什么是 Skills？

简单来说，你可以把 Skills 理解为一种**可动态加载的“能力包”**。

如果把 Agent 比作一个刚入职的员工，Tools 只是给他发了一把改锥（API），而 Skills 则是**“改锥 + 维修手册 + 老师傅的经验”**的合体。

从形式上看，Skill 就是一个文件夹，里面井井有条地组织了“脑”和“手”：
*   **指令 (SOP)**：写在 `SKILL.md` 里，告诉 Agent 遇到这个问题该第一步做什么，第二步做什么。
*   **脚本 (Scripts)**：确定的 Python/Bash 代码，用来执行计算、文件读写等硬操作。
*   **资源 (Resources)**：需要的模板、配置文件等。

![skill是什么-1.webp](https://image.imak.top/2026/01/89cec3952990abf85cbd4f642d110df3.webp)

## 为什么要发明 Skills？

在 Skills 出现之前，我们在开发 Agent 时主要面临着“扩容”和“稳定”的矛盾。Skills 的出现，主要是为了解决三个核心问题：

### 1. 上下文瓶颈

在**没有 Skills 之前**，我们要写一个复杂的 Agent，就像是在写一个巨大的单体应用（Monolithic）。所有的业务逻辑、50 个工具的定义、各种 Edge Case 的处理，都要一股脑塞满 System Prompt。

结果就是：**Context Window 瞬间爆炸**。不仅烧钱，更重要的是 token 太多，模型注意力分散，越跑越笨，更容易产生幻觉。

**有了 Skills 之后**，我们引入了一个关键理念：**渐进式披露 (Progressive Disclosure)**。

这就好比一种**“懒加载” (Lazy Loading)** 技术：
*   **启动时**：Agent 只知道有很多 Skill 的名字（目录），比如“合同审核包”、“数据清洗包”，占用的上下文极小。
*   **运行时**：只有当接到“审核这个合同”的任务时，Agent 才会去读取那个文件夹里详细的 `SKILL.md` 和代码。

> **渐进式披露，极大地扩展了 Agent 的能力边界，同时让单次推理的“能耗”保持在低位。**

![skill渐进式披露的三个阶段.webp](https://image.imak.top/2026/01/01bada280163aeaa1b529e8cdfedf8ea.webp)

### 2. 复用性

*   **以前**：你想做一个“合同审核 Agent”，得从头写 Prompt、调试 Tools。下次要做个“发票处理 Agent”，又得重来一遍。工作极其碎片化。
*   **现在**：你把“如何审核合同”这套**SOP（标准作业程序）** 打包成一个 Skill 文件夹。这个文件夹就是你的**标准交付物**。

任何一个通用的 Agent，只要挂载了这个文件夹，瞬间就拥有了这项专业能力。这实现了真正的“**一次编写，到处挂载**”。

### 3. 稳定性/确定性

LLM 是概率模型，天生带有随机性。对于数学计算、精确查询这类任务，完全依赖 LLM 推理就像在赌博。

Skills 允许你把 **确定性的代码** 和 **非确定性的推理** 绑在一起。
*   遇到逻辑推理，用 Prompt。
*   遇到排序、计算、IO，Skill 会强制 Agent 直接运行 Python 脚本，而不是自己傻傻地生成 Token 来模拟。

**一句话总结：用代码的“确定性”去弥补 LLM 的“随机性”。**

## Skills vs Tools：究竟有什么不同？

很多开发者容易把这两个概念混淆。其实，**Tools 是原子化的“动作”，而 Skills 是封装了“动作 + 策略 + 知识”的完整“模块”。**

我们可以从三个维度来看两者的差异：

### 1. 从“全量加载”变为“按需加载”
*   **Tools**：往往需要把所有工具的定义（JSON Schema）全塞进 Prompt。工具一多，Agent 就“晕”了。
*   **Skills**：基于目录的按需加载。这让 Agent 理论上可以挂载成百上千个能力，而始终保持头脑清醒。

### 2. 从“只有锤子”变为“锤子 + 说明书”
*   **Tools**：只给 Agent 一个 `execute_python` 的工具。Agent 知道能用，但不知道在复杂业务场景下（比如金融风控）该先查谁、后查谁。
*   **Skills**：不仅包含了代码（工具），还通过 markdown 文档包含了**专家流程（SOP）**。
> **Skills 把你的“专家经验” (Know-How) 变成了代码的一部分。**

### 3. 从“硬编码”变为“模块化分发”
*   **Skills** 的设计极其类似 Docker 镜像。它是一个独立的文件系统，**可移植性极强**。这为未来 Agent 社区的能力共享和标准化铺平了道路。

## 总结

Skills 的出现，标志着 Agent 开发从“手搓 Prompt”的作坊时代，迈向了**标准化、模块化**的软件工程时代。

它并没有取代 Tools，而是通过**更高层级的架构组织**，让 Tools 发挥出了更大的威力。

如果你**正在构建复杂的 Agent 应用**，现在就是时候把你的业务逻辑重构为 Skills 了。

##
推荐阅读

*   [**Equipping agents for the real world with Agent Skills**](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
    *   *Anthropic 官方首发 Skills 概念，详细阐述了"渐进式披露"的设计哲学。*
*   [**Cursor 的 Skills 介绍**](https://cursor.com/cn/docs/context/skills)
    *   *实战指南：如何在 Cursor 中配置和使用 Skills。*
*   [**Antigravity Skills 指南**](https://codelabs.developers.google.com/getting-started-with-antigravity-skills)
    *   *如何在 Google Antigravity 中构建和使用 Skills。*
*   [**Trae Skills 指南**](https://docs.trae.ai/ide/skills)
    *   *如何在 Trae 国际版中构建和使用 Skills。*
---

感谢阅读，如果这篇内容对你有启发，欢迎点赞、转发和关注支持。
