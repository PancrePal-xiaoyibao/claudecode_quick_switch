# Claude 服务商和模型切换工具

这是一个用于管理和切换不同 Claude API 服务商及其模型的 Shell 脚本工具。

## 功能特点

- 支持多个服务商切换
- 每个服务商支持独立的模型列表
- 简单的命令行界面
- 自动配置环境变量

## 支持的服务商

1. **Anyrouter**
   - Base URL: https://7a61fbe1b5f3.d93a09b6.top/
   - 支持多个 Claude 模型版本
   - 默认服务商

2. **Moonshot**
   - Base URL: https://api.moonshot.cn/anthropic
   - 支持 Kimi 模型

3. **GLM**
   - Base URL: https://open.bigmodel.cn/api/anthropic
   - 支持多个 GLM-4.5 系列模型（匹配haiku/sonnet/opus=4.5-air/4.5/4.6）

4. **Siliconflow**
   - Base URL: https://api.siliair/4.5/4.6
   - 支持 Kimi-K2-Instruct 模型

## 可用模型

### Anyrouter 模型
- default: claude-sonnet-4-20250514
- advanced: claude-3-7-sonnet-20250219
- enhanced: claude-sonnet-4-5-20250929
- haiku: claude-3-5-haiku-20241022
- sonnet: claude-3-5-sonnet-20241022

### Moonshot 模型
- default: kimi-k2-0711-preview

### GLM 模型（官方配置）
- haiku: glm-4.5-air (ANTHROPIC_DEFAULT_HAIKU_MODEL)
- sonnet: glm-4.5 (ANTHROPIC_DEFAULT_SONNET_MODEL)
- opus: glm-4.6 (ANTHROPIC_DEFAULT_OPUS_MODEL)
- default: glm-4.5 (默认使用 sonnet 模型)

### Siliconflow 模型
- default: moonshotai/Kimi-K2-Instruct

## 使用方法

1. **初始化环境**
   ```bash
   source claude_switch.sh
   ```

2. **切换服务商**
   ```bash
   switch_provider <服务商名称>
   ```
   可用的服务商名称：
   - anyrouter
   - moonshot
   - glm
   - siliconflow

3. **查看当前服务商支持的模型**
   ```bash
   show_models
   ```

4. **切换模型**
   ```bash
   switch_model <模型名称>
   ```

## 使用示例

1. 切换到 Moonshot 服务商：
   ```bash
   switch_provider moonshot
   ```

2. 查看当前可用模型：
   ```bash
   show_models
   ```

3. 切换到 Anyrouter 并使用高级模型：
   ```bash
   switch_provider anyrouter
   switch_model advanced
   ```

## 环境变量

脚本会自动设置以下环境变量：
- `ANTHROPIC_BASE_URL`: API 服务地址
- `ANTHROPIC_AUTH_TOKEN`: 认证令牌
- `ANTHROPIC_MODEL`: 当前使用的模型

## 注意事项

1. 切换服务商时会自动设置该服务商的默认模型
2. 每个服务商支持的模型不同，切换前请使用 `show_models` 查看可用选项
3. 环境变量仅在当前终端会话中有效