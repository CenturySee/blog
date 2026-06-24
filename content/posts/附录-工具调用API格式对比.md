# 附录：工具调用 API 格式对比 —— Anthropic vs OpenAI Chat Completions

> 背景：第三讲分析了 harness 如何拼装 `{ system, messages, tools }` 三槽；第五讲说明了 `tool_result` 必须保留结构（id 配对）的原因。本附录补全"同一工具调用在两家 API 里长什么样"，便于对照理解 harness 的格式选择。

## 1. 请求（工具定义）

**Anthropic**

```json
{
  "tools": [
    {
      "name": "read_file",
      "description": "读取文件内容",
      "input_schema": {
        "type": "object",
        "properties": {
          "file_path": { "type": "string" },
          "offset":    { "type": "integer" },
          "limit":     { "type": "integer" }
        },
        "required": ["file_path"]
      }
    }
  ]
}
```

**OpenAI Chat Completions**

```json
{
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "read_file",
        "description": "读取文件内容",
        "parameters": {
          "type": "object",
          "properties": {
            "file_path": { "type": "string" },
            "offset":    { "type": "integer" },
            "limit":     { "type": "integer" }
          },
          "required": ["file_path"]
        }
      }
    }
  ]
}
```

OpenAI 比 Anthropic 多套一层 `{ type: "function", function: {...} }`，字段名也不同（`parameters` vs `input_schema`）。

---

## 2. 模型决定调工具时的响应

**Anthropic**

```json
{
  "role": "assistant",
  "content": [
    { "type": "text", "text": "我来看一下第 400 行附近的内容。" },
    {
      "type": "tool_use",
      "id": "toolu_01ABC",
      "name": "read_file",
      "input": { "file_path": "query.ts", "offset": 390, "limit": 40 }
    }
  ],
  "stop_reason": "tool_use"
}
```

**OpenAI**

```json
{
  "choices": [{
    "message": {
      "role": "assistant",
      "content": null,
      "tool_calls": [
        {
          "id": "call_abc123",
          "type": "function",
          "function": {
            "name": "read_file",
            "arguments": "{\"file_path\": \"query.ts\", \"offset\": 390, \"limit\": 40}"
          }
        }
      ]
    },
    "finish_reason": "tool_calls"
  }]
}
```

关键差异：OpenAI 的 `arguments` 是**JSON 序列化后的字符串**，需要手动 `JSON.parse()`；Anthropic 的 `input` 直接是对象。

---

## 3. 把工具结果喂回模型

**Anthropic**：tool_result 嵌在 `role: "user"` 的 content 数组里。

```json
{
  "role": "user",
  "content": [
    {
      "type": "tool_result",
      "tool_use_id": "toolu_01ABC",
      "content": "390: snipCompactIfNeeded...\n400: microCompactIfNeeded..."
    }
  ]
}
```

**OpenAI**：工具结果有独立的 `role: "tool"`。

```json
{
  "role": "tool",
  "tool_call_id": "call_abc123",
  "content": "390: snipCompactIfNeeded...\n400: microCompactIfNeeded..."
}
```

---

## 4. 差异汇总

| 差异点 | Anthropic | OpenAI Chat Completions |
|---|---|---|
| 工具定义包装 | 直接 `{ name, description, input_schema }` | 套一层 `{ type: "function", function: {...} }` |
| schema 字段名 | `input_schema` | `parameters` |
| 模型调工具的载体 | `content[]` 里的 `tool_use` 块（可与 text 混排） | `tool_calls[]` 独立字段，`content: null` |
| 工具参数格式 | 直接 JSON object | JSON 序列化的**字符串**（需二次 parse） |
| 工具结果的 role | `user`（嵌在 content 数组里） | `tool`（独立 role） |
| stop_reason | `"tool_use"` | `"tool_calls"` |
| 并发多工具结果 | 同一条 user 消息的 content 数组里并列多个 tool_result | 多条 role:tool 消息依次追加 |

---

## 5. 为什么 Anthropic 用 role:user 而不是独立 role

Anthropic 的消息格式只有 `user` / `assistant` 两种角色，严格交替。tool_result 从语义上属于"环境/harness 侧把执行结果喂给模型"，归入 user 侧合理。

副作用：一条 user 消息可以在 content 数组里同时包含多个 `tool_result`（并发工具调用时），还能混入普通 `text` 块——这正是第三讲 `getAttachmentMessages` 把文件变更、记忆 prefetch 等作为 text 块注入同一条 user 消息的基础。

> 关联：第三讲（messages 槽的拼装与 tool_result 配对要求）、第五讲（microcompact 只挖空 tool_result 内容、不删 id 的原因）。
