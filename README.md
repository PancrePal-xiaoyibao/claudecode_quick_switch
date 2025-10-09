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
   - Base URL: https://api.siliconflow.cn/
   - 支持 Kimi-K2-Instruct 模型

5. **Dashscope**
   - Base URL: https://dashscope.aliyuncs.com/apps/anthropic
   - 支持 qwen-plus 和 qwen-flash 模型

6. **iFlow**
   - Base URL: https://apis.iflow.cn/v1/chat/completions
   - 支持多个模型（glm-4.6、deepseek-v3.1、kimi-k2-0905、qwen3-coder-plus）

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
- opus: glm-4.5 (ANTHROPIC_DEFAULT_OPUS_MODEL)
- default: glm-4.5 (默认使用 sonnet 模型)

### Siliconflow 模型
- default: moonshotai/Kimi-K2-Instruct

### Dashscope 模型
- default: qwen-plus
- fast: qwen-flash

### iFlow 模型
- default: glm-4.6
- sonnet: deepseek-v3.1
- haiku: kimi-k2-0905
- opus: qwen3-coder-plus

## 安装配置

1. **复制配置文件**
   ```bash
   # 创建工作目录（如果不存在）
   mkdir -p ~/claude_code

   # 复制模板文件
   cp cc_llm_quick_switch.sh.template ~/claude_code/cc_llm_quick_switch.sh
   ```

2. **配置环境变量**
   
   根据你使用的 Shell，选择相应的配置方式：

   **对于 zsh 用户（macOS 默认）**：
   ```bash
   # 编辑 .zshrc
   vim ~/.zshrc

   # 添加以下配置（复制 zshrc_snippet.template 的内容）
   export CLAUDE_SWITCH_SCRIPT="$HOME/claude_code/cc_llm_quick_switch.sh"

   # 各服务商密钥（替换为你的密钥）
   export ANYROUTER_API_KEY="your-anyrouter-key"
   export MOONSHOT_API_KEY="your-moonshot-key"
   export GLM_API_KEY="your-glm-key"
   export SILICONFLOW_API_KEY="your-siliconflow-key"
   export DASHSCOPE_API_KEY="your-dashscope-key"
   export IFLOW_API_KEY="your-iflow-key"

   # 自动加载脚本
   [[ -f "$CLAUDE_SWITCH_SCRIPT" ]] && {
     source "$CLAUDE_SWITCH_SCRIPT"
     # 设置默认服务商为 iflow
     llm_provider iflow
   }

   # 加载新配置
   source ~/.zshrc
   ```

   **对于 bash 用户**：
   ```bash
   # 编辑 .bashrc 或 .bash_profile
   vim ~/.bashrc  # Linux
   # 或
   vim ~/.bash_profile  # macOS

   # 添加与 zsh 相同的配置内容
   # 注意：bash 中 [[ ]] 语法同样有效

   # 加载新配置
   source ~/.bashrc  # Linux
   # 或
   source ~/.bash_profile  # macOS
   ```

## 使用方法

1. **切换服务商**
   ```bash
   llm_provider <服务商名称>
   ```
   可用的服务商：
   - iflow（默认）
   - anyrouter
   - moonshot
   - glm
   - siliconflow
   - dashscope

2. **查看当前服务商支持的模型**
   ```bash
   llm_show
   ```

3. **切换模型**
   ```bash
   llm_model <模型名称>
   ```
   例如：`llm_model default`、`llm_model sonnet`

4. **查看当前环境**
   ```bash
   llm_env
   ```
   显示当前服务商、API 地址和使用的模型

## 使用示例

1. 切换到 iFlow 服务商并使用默认模型：
   ```bash
   llm_provider iflow
   ```

2. 查看当前可用模型：
   ```bash
   llm_show
   ```

3. 切换到 sonnet 模型：
   ```bash
   llm_model sonnet
   ```

## 环境变量说明

脚本会自动设置以下环境变量：
- `CURRENT_PROVIDER`: 当前使用的服务商
- `ANTHROPIC_BASE_URL`: API 服务地址
- `ANTHROPIC_AUTH_TOKEN`: 认证令牌（从环境变量读取）
- `ANTHROPIC_MODEL`: 当前使用的模型

## 注意事项

1. 切换服务商时会自动设置该服务商的默认模型
2. 每个服务商支持的模型不同，切换前请使用 `llm_show` 查看可用选项
3. 环境变量仅在当前终端会话中有效
4. API 密钥通过环境变量管理，确保安全性
5. 建议将 `iflow` 设置为默认服务商，可以在 Shell 配置文件中设置