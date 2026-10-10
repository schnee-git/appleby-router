# MVP-PRD

## 1. 项目目标

开发一个 AI 模型中转站（AI Gateway）。

开发者通过统一 API 调用多个 AI 厂商的大模型，无需分别对接各厂商 API。

---

# 2. MVP 功能范围

## 2.1 Provider 管理

管理 AI 服务提供商。

MVP 支持：

- OpenAI
- Anthropic（Claude）
- DeepSeek
- 智谱 AI（GLM）

每个 Provider 配置：

- Provider 名称
- API Key
- Base URL（可选）
- 是否启用

---

## 2.2 Model 管理

维护平台支持的模型列表。

每个模型绑定一个 Provider。

例如：

| Model | Provider |
|--------|----------|
| gpt-5.5 | OpenAI |
| gpt-5.5-mini | OpenAI |
| claude-sonnet-4 | Anthropic |
| deepseek-chat | DeepSeek |
| glm-4.5 | 智谱 |

支持：

- 查询模型列表
- 启用/禁用模型

---

## 2.3 Chat Completion

提供统一聊天接口。

开发者指定：

- model
- messages

Gateway 自动将请求路由到对应 Provider。

支持：

- 普通响应
- Streaming（SSE）

---

## 2.4 API Key

开发者通过 API Key 调用 Gateway。

支持：

- 创建 API Key
- 查询 API Key
- 删除 API Key
- 禁用 API Key

---

## 2.5 Authentication

所有 API 必须进行身份认证。

支持：

- Bearer API Key
- API Key 有效性校验
- API Key 是否禁用校验

非法请求直接返回 Unauthorized。

---

## 2.6 Usage

记录每次模型调用。

记录内容：

- User
- API Key
- Provider
- Model
- Prompt Tokens
- Completion Tokens
- Total Tokens
- Cost
- Status
- Request Time
- Duration

支持：

- 查询 Usage 列表
- 查询 Usage 明细

---

## 2.7 Billing

平台根据模型价格自动计算调用费用。

支持：

- 查询账户余额
- 查询累计消费
- 查询消费记录
- 自动计算调用费用
- 自动扣减余额
- 余额不足拒绝调用

费用计算依据：

- Provider
- Model
- Prompt Tokens
- Completion Tokens

---

## 2.8 Pricing

维护模型价格。

每个模型维护：

- Prompt Price
- Completion Price
- 计费单位（Per 1M Tokens）

支持：

- 查询价格
- 修改价格

---

## 2.9 Provider Router

根据请求中的 Model 自动选择对应 Provider。

例如：

| Model | Route |
|--------|-------|
| gpt-5.5 | OpenAI |
| deepseek-chat | DeepSeek |
| claude-sonnet-4 | Anthropic |
| glm-4.5 | 智谱 |

MVP Router 仅负责模型映射。

不支持：

- Failover
- Retry
- Load Balance
- Weight
- Intelligent Routing

---

## 2.10 Rate Limit

限制 API 调用频率。

支持：

- 按 API Key 限流

MVP：

- 固定限流策略

---

# 3. 用户角色

## Developer

权限：

- 管理 API Key
- 调用模型
- 查询模型列表
- 查询 Usage
- 查询 Billing
- 查询 Balance

---

## Admin

权限：

- 管理 Provider
- 管理模型
- 管理价格
- 查看系统数据

---

# 4. API

## Authentication

- POST /apikeys
- GET /apikeys
- DELETE /apikeys/{id}

---

## Chat

- POST /v1/chat/completions

---

## Models

- GET /v1/models

---

## Usage

- GET /usage
- GET /usage/{id}

---

## Billing

- GET /balance
- GET /billing

---

## Pricing

- GET /pricing

---

## Provider（Admin）

- GET /providers
- POST /providers
- PUT /providers/{id}

---

## Model（Admin）

- POST /models
- PUT /models/{id}

---

# 5. 非目标（MVP 不做）

## AI 能力

- Image Generation
- Audio
- Video
- Embedding
- Fine-tuning
- Responses API
- Assistant API
- Batch API

---

## 企业能力

- Team
- Workspace
- Organization
- RBAC
- SSO
- 审计日志

---

## Gateway 能力

- Provider 自动切换
- Retry
- Load Balance
- Prompt Cache
- Semantic Cache
- 智能模型推荐
- 多区域部署

---

## Billing

- 在线支付
- 自动充值
- 发票
- 优惠券
- 订阅套餐

---

# 6. MVP 完成标准

满足以下条件即完成 MVP：

- 支持 OpenAI、Anthropic、DeepSeek、智谱四个 Provider。
- 支持统一 Chat Completion API。
- 支持普通响应与 Streaming（SSE）。
- 支持 API Key 鉴权。
- 支持 Provider 管理。
- 支持 Model 管理。
- 支持查询模型列表。
- 支持 Usage 记录。
- 支持模型价格配置。
- 支持按 Token 自动计费。
- 支持余额管理。
- 支持自动扣减余额。
- 支持余额不足拦截。
- 支持查询消费记录。
- 支持 API Key 限流。
- 新增 Provider 时，仅需新增 Provider 实现及配置，无需修改核心业务流程。