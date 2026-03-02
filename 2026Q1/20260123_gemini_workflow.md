# Agent 进阶：Skills 才是 AI 的“外挂大脑”

大家好，我是算法工程笔记。

最近技术圈都在聊 Claude 推出的 Skills，但我发现不少朋友把它和 Tool Use 搞混了。

其实，如果说 Tools 是 Agent 的“手”，那 Skills 更像是让它拥有了**可随时插拔的“外挂大脑”**。今天这篇主要聊聊，为什么 Skills 会是 Agent 走向复杂工程化的关键一步。

---

## 什么是 Skills？

简单来说，这是一个**可动态加载**的“能力包”。

它不像以前那样把所有 Prompt 都写在系统提示词里，而是像一个工整的文件夹，里面整整齐齐地放着：
*   **SOP (Instructions)**：写好的标准作业程序。
*   **代码 (Scripts)**：执行具体任务的 Python 脚本。
*   **资料 (Resources)**：必要的辅助文件。

Agent 就像玩乐高一样，需要什么能力，就“咔嚓”一声把对应的 Skills 模块插上去，**按需加载**。

> **Skills = 动作 (Tools) + 策略 (SOP) + 知识 (Docs)**

## 为什么它很重要？

我们在做 Agent 开发时，最头疼的就是 Context（上下文）不够用。Skills 的出现，主要解决了两个核心痛点：

### 1. 渐进式披露：给大脑“减负”

**没有 Skills 前（单体模式）**：
我们需要把所有的业务逻辑、几十个 Tool 的定义，全部一股脑塞进 System Prompt 里。  
这就好比让一个员工在入职第一天，就把公司几百页的规章制度倒背如流。  
结果就是：**贵**（Token 消耗大）且 **笨**（模型容易分心）。

**有了 Skills 后（微服务模式）**：
Agent 采用了 **“渐进式披露” (Progressive Disclosure)** 的策略。
*   **平时**：Agent 只知道兜里揣着个锦囊叫“PDF处理技能”，不知道具体怎么用。
*   **用时**：只有当真的需要处理 PDF 时，它才会打开锦囊，动态加载里面的说明书。

**这种机制彻底打破了上下文的瓶颈，理论上你可以给 Agent 挂载成百上千个 Skills，而它依然轻装上阵。**

![skill渐进式披露的三个阶段.webp](https://image.imak.top/2026/01/01bada280163aeaa1b529e8cdfedf8ea.webp)

### 2. 拒绝“重复造轮子”

做过 Agent 的人都知道，调试 Prompt 是个体力活。

有了 Skills，你可以把“合同审核”这一套 **SOP + 脚本** 打包成一个 Skill 文件夹。这就是标准的交付物。任何通用的 Agent，只要挂载这个文件夹，瞬间就拥有了这项专家能力。

**一次编写，到处挂载**，就像我们管理 Python 库一样简单。

## Skills vs. Tools：到底强在哪？

很多同学容易混淆，这里做一个直观的对比。

> **Tools 是原子化的“动作”，而 Skills 是封装了“动作 + 策略 + 知识”的完整“模块”。**

*   **Tools 就像 API 函数** (如 `requests.get`)，它只是一个执行动作。
*   **Skills 就像封装好的 Library**，它不仅有了底层的 API，还包含了文档说明、最佳实践，以及配套的代码逻辑。

它并没有取代 Tools，而是通过把 **确定性的代码** (Scripts) 和 **非确定性的推理** (Prompt) 强绑定，大幅提升了 Agent 处理复杂任务的成功率。

## 推荐阅读

*   [**Equipping agents for the real world with Agent Skills**](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)：Anthropic 官方首发 Skills 概念，详细阐述了“渐进式披露”的设计哲学。
*   [**Cursor 的 Skills 介绍**](https://cursor.com/cn/docs/context/skills)：实战指南，教你如何在 Cursor 中配置和使用 Skills。
*   [**Antigravity Skills 指南**](https://codelabs.developers.google.com/getting-started-with-antigravity-skills)：如何在 Google Antigravity 中构建和使用 Skills。

---
感谢阅读，如果这篇内容对你有启发，欢迎点赞、转发和关注支持。
