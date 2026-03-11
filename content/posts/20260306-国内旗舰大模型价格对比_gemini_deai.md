---
title: 七家国产大模型API价格横评，谁最值？
# cover: images/20260306-国内旗舰大模型价格对比_cover.png
author: AK27
source_url: https://api-docs.deepseek.com/zh-cn/quick_start/pricing
date: "2026-03-06"
draft: false
tags: ["大模型", "API价格", "成本", "性价比"]
categories: ["技术"]
ShowToc: true
---

大家好，我是算法工程笔记。

最近 OpenClaw（龙虾）挺火，不少同学试了试就被 token 用量吓到了。开源模型本身免费，但跑在 API 上一点不便宜。今天这篇主要聊聊**国产旗舰大模型的 API 价格**，把几家主流厂商摆到一起比一比，帮大家挑一个适合自己场景的。

> 这里只讨论 API 调用价格，不涉及各家的 Coding Plan。目前大多数厂商的 Coding Plan 都会使用用户数据做训练，隐私方面需要自己衡量。

---

## 七家厂商横向对比

| 厂商 | 模型名称 | 上下文长度 | 输入价格 (元/百万Tokens) | 输出价格 (元/百万Tokens) | 数据来源 |
| :--- | :--- | :---- | :--- | :--- | :--- |
| 字节 | doubao-seed-2.0-pro | 256K | **3.2-9.6** | **16-48** | [火山方舟](https://www.volcengine.com/docs/82379/1544106?lang=zh) |
| 阿里云 | Qwen3.5-Plus | 1M | **0.8** | **4.8** | [百炼平台](https://bailian.console.aliyun.com/) |
| DeepSeek | DeepSeek-V3.2 | 128K | **2** | **3** | [DeepSeek 定价](https://api-docs.deepseek.com/zh-cn/quick_start/pricing) |
| 百度智能云 | ERNIE 5.0 | 128K | **6-10** | **24-40** | [千帆计费](https://cloud.baidu.com/doc/qianfan/s/wmh4sv6ya) |
| 智谱AI | GLM-5 | 200K | **4-6** | **18-22** | [智谱定价](https://bigmodel.cn/pricing) |
| Kimi | Kimi K2.5 | 256K | **0.7-4** | **21** | [Kimi定价](https://platform.moonshot.cn/docs/pricing/chat) |
| MiniMax | MiniMax M2.5 | 192K | **2.1-4.2**（估算） | **8.4-16.8**（估算） | [MiniMax定价](https://platform.minimaxi.com/docs/guides/pricing-paygo) |

数字摆出来，差距很大。下面拆开来看。

## 输入价格：阿里和 Kimi 杀疯了

输入价格最低的两家是阿里的 Qwen3.5-Plus（**0.8 元/百万 Tokens**）和 Kimi K2.5（命中缓存 **0.7 元**，未命中 **4 元**）。DeepSeek-V3.2 命中缓存也只要 **0.2 元**，很夸张。

如果你的场景是长 prompt 加固定系统指令（比如 Agent 框架），缓存命中率高的话，DeepSeek 的输入成本几乎可以忽略。

字节的 doubao-seed-2.0-pro 走分段计费：32K 以内 3.2 元，32K-128K 涨到 4.8 元，128K 以上直接 9.6 元。用短文本还行，上下文一拉长就贵了。百度的 ERNIE 5.0 也差不多，短文本 6 元，长文本 10 元，在这批里算偏贵的。

## 输出价格：DeepSeek 断档领先

输出这边 DeepSeek-V3.2 只要 **3 元/百万 Tokens**，所有厂商里最低。阿里 Qwen3.5-Plus 排第二，**4.8 元**。

输出最贵的是字节 doubao-seed-2.0-pro，长文本档位下要 **48 元/百万 Tokens**，跟 DeepSeek 差了 16 倍。Kimi K2.5 的输出价格固定 **21 元**，中等偏上。

> 输出 token 一般比输入贵，因为生成需要的算力更大。批量生成任务里，输出价格才是大头。

## 上下文长度：阿里 1M 独一档

阿里 Qwen3.5-Plus 支持 **1M** 上下文，这批里没有对手。其次是字节和 Kimi，都是 256K。DeepSeek 和百度只有 128K。

要处理超长文档的话，目前只有阿里能打。

## 怎么选？按场景来

* 跑量、批量生成：DeepSeek-V3.2，输入输出都最便宜，128K 上下文够用。
* 长文档处理：Qwen3.5-Plus，1M 上下文，价格也不贵。
* 带缓存的 Agent 场景：DeepSeek 缓存命中 0.2 元输入，适合反复调用固定 prompt 的场景。Kimi 的缓存价格（0.7 元）也可以。
* 追求模型能力、预算充足：字节 doubao-seed-2.0-pro 和百度 ERNIE 5.0，价格高但各有特色，看业务需求。

一句话总结：**要便宜选 DeepSeek，要长上下文选阿里，其他看需求**。

---

感谢阅读，如果这篇内容对你有启发，欢迎点赞、转发和关注支持。
