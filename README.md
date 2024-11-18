# notdiamond2api

这是一个基于 Cloudflare Workers 的聊天代理服务，用于将请求转发到 chat.notdiamond.ai 服务器。

## 功能特点

- 支持多种 AI 模型的映射
- 处理流式和非流式响应
- 兼容 OpenAI API 格式
- 支持多账号配置和自动轮询
- 自动登录和令牌管理
- Token 失效自动刷新
- 账号异常自动切换
- 一键无忧部署启动

## 支持的模型

目前支持以下模型：

- gpt-4o
- gpt-4-turbo-2024-04-09
- gpt-4o-mini
- claude-3-5-haiku-20241022
- claude-3-5-sonnet-20241022
- gemini-1.5-pro-latest
- gemini-1.5-flash-latest
- Meta-Llama-3.1-70B-Instruct-Turbo
- Meta-Llama-3.1-405B-Instruct-Turbo
- llama-3.1-sonar-large-128k-online
- mistral-large-2407

## 部署配置

### Cloudflare Workers 环境变量配置

必需的环境变量：
- `ACCOUNTS_CONFIG`：JSON 格式的账号配置数组，例如：
  ```json
  [
    {"email": "account1@example.com", "password": "password1"},
    {"email": "account2@example.com", "password": "password2"}
  ]
  ```
  或者使用单账号配置：
- `AUTH_EMAIL`：登录邮箱
- `AUTH_PASSWORD`：登录密码

可选的环境变量：
- `AUTH_ENABLED`：是否启用 API 访问验证（true/false）
- `AUTH_VALUE`：API 访问令牌（当 AUTH_ENABLED 为 true 时需要配置）

## API 接口

### 模型列表
```http
GET /v1/models
```
返回所有可用的模型列表。

### 聊天完成
```http
POST /v1/chat/completions
```
发送聊天完成请求，支持流式和非流式响应。

## 特性说明

### 多账号支持
- 支持配置多个账号，系统会自动在账号间轮询
- 当某个账号请求失败时，自动切换到下一个账号
- 保持每个账号的独立状态管理

### 自动重试机制
1. 首次请求失败时，尝试刷新令牌
2. 令牌刷新失败时，尝试重新登录
3. 当前账号完全失败时，自动切换到下一个账号
4. 直到找到可用账号或所有账号都尝试失败

## 更新日志

### 2024-11-13
1. 添加多账号支持
2. 优化账号轮询机制
3. 改进错误处理和重试逻辑

### 2024-11-12
1. 更新模型映射

[之前的更新日志...]

## 许可证

本项目仅供学习使用，24小时后请删除。不得用于商业或其他目的。
