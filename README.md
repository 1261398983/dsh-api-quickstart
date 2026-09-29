# AI API Gateway Quickstart

> 一份把 Claude / GPT / Gemini 收进单个 API Key 的完整配置示例，覆盖 6 种主流工具。

**统一端点**

```
https://api.dshapi.icu/v1
```

兼容 OpenAI 与 Anthropic 双协议。改一行 `base_url`，工具零改造。

**[→ 在线配置指南](https://1261398983.github.io/ai-api-guide/)**

---

## 目录

- [Claude Code](#claude-code)
- [Codex CLI](#codex-cli)
- [Cherry Studio](#cherry-studio)
- [NextChat / LobeChat](#nextchat--lobechat)
- [Python (OpenAI SDK)](#python-openai-sdk)
- [Node.js](#nodejs)
- [curl 自测](#curl-自测)
- [常见报错对照](#常见报错对照)

---

## Claude Code

```bash
# Windows PowerShell
$env:ANTHROPIC_BASE_URL="https://api.dshapi.icu/v1"
$env:ANTHROPIC_AUTH_TOKEN="你的Key"
claude

# macOS / Linux
export ANTHROPIC_BASE_URL="https://api.dshapi.icu/v1"
export ANTHROPIC_AUTH_TOKEN="你的Key"
claude
```

## Codex CLI

环境变量方式：

```bash
export OPENAI_BASE_URL="https://api.dshapi.icu/v1"
export OPENAI_API_KEY="你的Key"
codex
```

或持久化到 `~/.codex/config.toml`：

```toml
model_provider = "dshapi"

[model_providers.dshapi]
name = "DSH API"
base_url = "https://api.dshapi.icu/v1"
env_key = "OPENAI_API_KEY"
```

## Cherry Studio

`设置` → `模型服务` → `添加提供商`

| 字段 | 值 |
|---|---|
| 类型 | OpenAI（或 Anthropic） |
| API 地址 | `https://api.dshapi.icu/v1` |
| 密钥 | 你的 Key |

点「检查」自动拉取模型列表。

## NextChat / LobeChat

`设置` → `自定义接口`

- 接口地址：`https://api.dshapi.icu/v1`
- API Key：你的 Key

## Python (OpenAI SDK)

```python
from openai import OpenAI

client = OpenAI(
    api_key="你的Key",
    base_url="https://api.dshapi.icu/v1",
)

resp = client.chat.completions.create(
    model="claude-sonnet-4-5",
    messages=[{"role": "user", "content": "你好"}],
)
print(resp.choices[0].message.content)
```

## Node.js

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "你的Key",
  baseURL: "https://api.dshapi.icu/v1",
});

const resp = await client.chat.completions.create({
  model: "claude-sonnet-4-5",
  messages: [{ role: "user", content: "你好" }],
});
console.log(resp.choices[0].message.content);
```

## curl 自测

```bash
curl https://api.dshapi.icu/v1/chat/completions \
  -H "Authorization: Bearer 你的Key" \
  -H "Content-Type: application/json" \
  -d '{"model":"claude-sonnet-4-5","messages":[{"role":"user","content":"hi"}]}'
```

---

## 常见报错对照

| 报错 | 原因 | 处理 |
|---|---|---|
| `401 Unauthorized` | Key 错误或未携带 | 检查 `Authorization` 头，确认 Key 完整无空格 |
| `404 Not Found` | base_url 缺或多写 `/v1` | 统一使用 `https://api.dshapi.icu/v1` |
| `model not found` | 模型名不匹配 | 后台查看当前可用模型列表 |
| 连接超时 | 本地代理干扰 | 关闭系统代理，或将端点加入代理白名单 |
| `insufficient quota` | 余额不足 | 后台充值（支持国内支付方式） |

---

## 选站 Checklist

- [ ] 双协议兼容（OpenAI + Anthropic），否则不同工具要分别适配
- [ ] 按量计费，余额不过期
- [ ] 有后台用量明细，可查每次调用 token
- [ ] 支付方式本地化
- [ ] 有稳定的售后响应渠道

---

## 相关资源

- **在线配置指南**：https://1261398983.github.io/ai-api-guide/
- **API 端点**：https://api.dshapi.icu/v1

价格与模型可用性以平台后台实际展示为准。
