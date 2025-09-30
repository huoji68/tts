# 高性能 Edge TTS EdgeOne Pages 代理

这是一个部署在 EdgeOne Pages 上的高性能文本转语音（TTS）代理服务。它巧妙地将微软 Edge 强大且自然的语音合成服务，封装成了一个兼容 OpenAI API 格式的接口。这使得开发者可以无缝地将各种现有应用对接到这个免费、高质量的 TTS 服务上。

项目包含两个核心部分：

1. `edge-functions/index.js`: 部署在 EdgeOne Pages 上的核心服务脚本（Edge Functions）
2. `index.html`: 一个功能完备的网页，用于方便地测试和调用服务

## 在线体验地址

需要海外访问，自部署后，可绑定自定义域名提供国内访问

https://fyjtjs.com/

---

## ✨ 功能亮点

- **🚀 OpenAI 兼容**: 完全模拟 OpenAI 的 `/v1/audio/speech` 接口，可被官方的 OpenAI SDK 或任何现有工具直接调用
- **🗣️ 高质量音色**: 利用微软 Edge TTS 提供的多种自然、流畅的神经网络语音
- **📡 STREAMING**: 支持**流式**和**标准**（非流式）两种响应模式，流式响应可极大降低长文本的首次播放延迟
- **🧠 智能文本清理**: 内置强大的"文本清理流水线"，可自动处理从 PDF 或网页复制的杂乱文本
- **🎛️ 灵活的参数配置**: 支持通过 API 请求动态调整所有核心参数
- **🌐 零依赖部署**: 脚本完全自包含，无需配置 KV、队列等任何外部服务
- **💻 便捷的测试工具**: 提供一个功能丰富的 `index.html`，让用户无需编写任何代码即可测试所有功能

---



#### 1. 标准请求

```bash
curl --location 'https://<你的域名>/api/v1/audio/speech' \
--header 'Authorization: Bearer hello' \
--header 'Content-Type: application/json' \
--data '{
    "model": "tts-1-alloy",
    "input": "你好，这是一个标准的语音合成请求。"
}' \
--output standard.mp3
```

#### 2. 流式请求 (用于长文本)

```bash
curl --location 'https://<你的域名>/api/v1/audio/speech' \
--header 'Authorization: Bearer hello' \
--header 'Content-Type: application/json' \
--data '{
    "model": "tts-1-nova",
    "input": "这是一个流式请求的示例，对于较长的文本，你能更快地听到声音的开头部分。",
    "stream": true
}' \
--output streaming.mp3
```

---

## 📁 项目结构说明

- **`edge-functions/api/v1/audio/speech.js`**: 核心 TTS API 处理逻辑
- **`edge-functions/api/v1/models.js`**: 模型列表 API 端点
- **`index.html`**: 前端测试页面，提供可视化界面来测试 API 功能


### 📂 完整文件结构

```
edgetts-edgeone-pages/
├── edge-functions/
│   └── api/
│       └── v1/
│           ├── models.js           # GET /api/v1/models
│           └── audio/
│               └── speech.js       # POST /api/v1/audio/speech
├── index.html                      # 前端测试页面
├── README-EdgeOne.md              # 详细说明文档
└── deploy.md                      # 快速部署指南
```

---

## ⚠️ 重要限制

- **字符数限制**: 单次请求的文本长度建议不超过 **12 万字符**
- **并发限制**: 默认并发数为 10，可根据需要调整
- **CPU 时间**: EdgeOne Pages Edge Functions 单次执行限制为 200ms CPU 时间
