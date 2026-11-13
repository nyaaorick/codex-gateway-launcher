# Codex Gateway Launcher

在**不影响官方 Codex 插件和 ChatGPT 登录**的前提下,让 VS Code 里的 Codex 图形界面走你自己的
OpenAI 兼容网关(例如 [Bifrost](https://github.com/maximhq/bifrost))。

Use the official Codex VS Code extension against your own OpenAI-compatible gateway, in a separate
VS Code instance, without touching your normal Codex / ChatGPT setup.

## 安装

1. 从 [Releases](https://github.com/nyaaorick/codex-gateway-launcher/releases/latest) 下载 `.vsix`。
2. 安装:
   ```bash
   code --install-extension codex-gateway-launcher-*.vsix
   ```
   或在扩展面板右上角菜单选择"从 VSIX 安装…"。
3. 确保已安装官方 Codex 插件(`openai.chatgpt`)。

## 使用

1. 点击左侧活动栏的"网关 Codex"图标。
2. 填写网关地址(以 `/v1` 结尾)和网关 key。保存后会自动测试连接。
3. 再点一次图标:打开一个独立的 VS Code 窗口,其中的 Codex 插件走网关。

之后每次点图标都会直接打开该实例。命令面板搜索"网关 Codex"可找到全部命令。

## 工作原理

- 启动第二个 VS Code 实例,使用独立的 `--user-data-dir` 和独立的 `CODEX_HOME`(默认 `~/.codex-gateway`)。
- 首次启动时在该目录生成 `config.toml`,其中定义一个自定义模型服务商(`wire_api = "responses"`)。
- key 保存在 VS Code SecretStorage(macOS 钥匙串等),只通过环境变量 `CODEX_GATEWAY_API_KEY` 传给新实例。
- 不读取、不修改 `~/.codex`,官方实例照常使用订阅登录。

## 要求

- 网关需支持 OpenAI Responses API(`POST /v1/responses`,流式)和 `GET /v1/models`。
- 网关实例是全新的 VS Code 配置(主题、快捷键为默认值);扩展与主实例共享。
- 只在 macOS 上测试过;Linux / Windows 有适配代码但未验证。

## 设置

| 设置 | 说明 |
|---|---|
| `codexGateway.baseUrl` | 网关地址,以 `/v1` 结尾 |
| `codexGateway.defaultModel` | 首次生成配置时写入的默认模型 |
| `codexGateway.codexHome` | 网关实例的 Codex 配置目录 |
| `codexGateway.launchOnIconClick` | 点击图标是否立即打开实例 |

## License

MIT
