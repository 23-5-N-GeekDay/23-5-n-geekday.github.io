---
title: AI Agent 赛道 · 赞助商资源启用指南
description: AI Agent 大模型智能体赛道赞助商提供的平台资源与详细启用步骤
date: 2026-02-18
---

# AI Agent 赛道 · 赞助商资源启用指南

> 以下资源由赞助商为本次极客节参赛选手专属提供，请在 Kick Off 前完成账号注册并等待下发相应code来激活。

<br>

---

<br>

## 🖥️ 算能科技 · SophNet API

**提供内容**：SophNet 平台 API 调用额度，涵盖大语言模型、视觉模型（文生视频、图生视频、文生图）等多类能力

### 启用步骤

1. **注册 SophNet 账号**
   - 访问 [sophnet.com](https://sophnet.com)
   - 使用手机号注册并完成验证

2. **获取 API Key**
   - 点击进入 [API Key 管理页面](https://sophnet.com/#/project/key?orgId=25322&prjId=49493) 创建你的 API Key
   - 联系组委会获取**极客节专属额度兑换码**并激活
   - **每人限额 100 CNY**

3. **调用大语言模型（Chat Completions）**

兼容 OpenAI SDK，`base_url` 替换为 SophNet 地址即可：

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_SOPHNET_API_KEY",
    base_url="https://www.sophnet.com/api/open-apis/v1"
)

response = client.chat.completions.create(
    model="Qwen2.5-72B-Instruct",  # 也可选 DeepSeek-v3 等
    messages=[
        {"role": "system", "content": "你是一个有用的AI助手"},
        {"role": "user", "content": "帮我规划一个 AI Agent 的架构"}
    ]
)
print(response.choices[0].message.content)
```

4. **Function Calling（工具调用）**

SophNet 支持标准 Function Calling，适合构建 AI Agent 工具链：

```python
response = client.chat.completions.create(
    model="DeepSeek-v3",
    messages=[{"role": "user", "content": "今日上海天气如何？"}],
    tools=[{
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "获取指定城市的当前天气",
            "parameters": {
                "type": "object",
                "properties": {
                    "latitude": {"type": "number"},
                    "longitude": {"type": "number"}
                },
                "required": ["latitude", "longitude"]
            }
        }
    }]
)
# 从 response.choices[0].message.tool_calls 获取函数调用信息
```

5. **视觉模型：文生视频 / 图生视频**

SophNet 提供异步视频生成接口，支持万相、字节豆包、生数等系列模型：

```bash
# 文生视频（Wan2.6-T2V）
curl -X POST "https://www.sophnet.com/api/open-apis/projects/easyllms/videogenerator/generate" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Wan2.6-T2V",
    "content": [{"type": "text", "text": "一只可爱的小猫在阳光下玩耍"}],
    "parameters": {"duration": 5, "size": "1280*720", "fps": "24", "watermark": false}
  }'
# 返回 task_id，用于查询生成状态
```

```bash
# 图生视频（Wan2.6-I2V）
curl -X POST "https://www.sophnet.com/api/open-apis/projects/easyllms/videogenerator/generate" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Wan2.6-I2V",
    "content": [
      {"type": "image_url", "image_url": {"url": "https://example.com/reference.jpg"}},
      {"type": "text", "text": "让图片中的人物动起来"}
    ],
    "parameters": {"duration": 5, "resolution": "720p", "watermark": false}
  }'
```

> 📖 完整 API 文档：[sophnet.com/docs/component/API.html](https://sophnet.com/docs/component/API.html) | [视觉模型文档](https://sophnet.com/docs/component/vision_model.html)

<br>

---

<br>

## 🛠️ TRAE · TraePro 会员

> 📌 **注意**：TRAE 目前为**海外版本**，访问 trae.ai 及使用客户端需要**科学上网**，请提前准备好网络环境。会员权益通过组委会提供的**兑换码**激活。

**提供内容**：每位选手免费获得 **1个月 TraePro 会员**

### 启用步骤

1. **下载 TRAE**
   - 访问 [trae.ai](https://trae.ai) 下载客户端
   - 支持 macOS / Windows / Linux

2. **注册并激活极客节专属会员**
   - 使用邮箱注册 TRAE 账号
   - 打开 TRAE → 点击右上角头像 → 「Subscription」
   - 输入组委会提供的**兑换码**完成激活
   - 会员有效期：活动期间 + 30天

3. **TraePro 核心功能**
   - **AI 代码补全**：基于项目上下文的智能补全，支持中文注释
   - **Agent 模式**：自动拆解任务、调用工具、执行多步骤工作流
   - **知识库连接**：接入本地文档/代码库作为 RAG 数据源
   - **MCP 连接器**：一键接入 MCP 广场工具（天气、搜索、数据库等）


> 💡 **使用建议**：在 TRAE 中打开你的项目文件夹，让 AI 了解整个代码库上下文，补全效果会更准确。

<br>

---

<br>

## 🎞️ [Tosea.ai](https://tosea.ai) · AI Slides

**提供内容**：AI 智能幻灯片生成工具使用权限，助力高效制作演示文稿

### 启用步骤

1. **访问 [Tosea.ai](https://tosea.ai)**
   - 使用邮箱注册账号

2. **激活极客节权益（通过 Stripe）**
   - 登录后点击右上角头像 → 「Upgrade」或「Billing」
   - 在 Stripe 付款页面找到 **「Add promotion code」**
   - 输入组委会提供的 **Stripe Promotion Code** 完成兑换
   - 成功后页面显示折扣为 100%，无需付款

3. **生成项目演示文稿**
   - 点击「新建演示」→「AI 生成」
   - 输入项目描述（支持中文），例如：
     > "一个基于大模型的智能作业辅导 Agent，能够分析学生错题并生成个性化学习计划"
   - AI 自动生成结构化幻灯片（含标题、内容、图表建议）
   - 支持手动编辑、主题切换
   - 支持导出为 **PDF / PPTX**

> 🎯 **使用建议**：在 Hackathon 最后阶段，用 **[Tosea.ai](https://tosea.ai)** 快速生成项目演示文稿，节省时间专注于技术实现。

<br>

---

<br>

## 📋 资源申请清单

在 Kick Off 前，请完成以下准备工作：

- [ ] 注册 SophNet 账号，并向组委会申请算力兑换码
- [ ] 下载 TRAE 客户端，使用组委会提供的**兑换码**激活 TraePro 会员
- [ ] 注册 [Tosea.ai](https://tosea.ai) 账号，使用组委会提供的 **Stripe Promotion Code** 激活会员

<br>

---

**遇到问题？** 联系组委会：cysybeijing@163.com
