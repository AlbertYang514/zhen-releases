# 圳（Zhen）

[简体中文](#简体中文) · [English](#english)

[官方发行页 / Official Releases](https://github.com/AlbertYang514/zhen-releases/releases) · [反馈 / Feedback](https://github.com/AlbertYang514/zhen-releases/issues)

## 简体中文

圳（Zhen Harness）是一个本地优先的 Coding + Work AI Agent Harness 项目，目标是让用户在自己选择的模型服务与授权范围内使用 Agent 完成编程和日常工作。模型 API 由用户自行配置（BYOK）；圳不提供作者运营的模型 API。

项目规划包含命令行（CLI）与桌面端（Desktop）。CLI 当前以 `zhen` 为命令名。**本仓库目前还没有公开的、经过验收的安装包或二进制 Release；桌面安装包和 CLI 下载暂不可用。**请关注上方官方发行页，不要从非官方镜像获取所谓安装包。

### 客户端与平台状态

首版 Beta 的计划打包目标如下，均不代表已发布或完成平台验收：

| 客户端 / 目标平台 | 状态 |
| --- | --- |
| CLI：macOS Apple Silicon（arm64） | 计划目标；尚无公开验收构建 |
| CLI：Windows（x64） | 计划目标；尚无公开验收构建 |
| CLI：Linux（x86_64） | 计划目标；尚无公开验收构建 |
| Desktop：macOS Apple Silicon / Windows x64 / Linux x86_64 | 计划中的客户端；本仓库尚无可下载安装包，也未在此声明平台验收 |

### 安装与首次配置

当前没有可供安装的官方二进制包。**不要按下面的配置示例推断已经有可下载 CLI。**当官方 Release 发布验收过的客户端后，请从该 Release 选择对应的 CLI 或 Desktop 资产；Runtime Bundle 是执行运行时资源，不是 Desktop 安装器。

首版 CLI 的配置流程设计为使用 `zhen config init` 生成配置模板，再把 API Key 放入模板指定的环境变量。当前 CLI 使用 OpenAI-compatible Chat Completions 接口。拿到官方 CLI 后，先运行 `zhen --help` 查看该构建支持的命令。

配置格式示例（占位值，不含真实凭据）：

```toml
[model]
provider = "openai-compatible"
base_url = "https://your-endpoint.example/v1"
model = "your-model-id"
api_key_env = "ZHEN_API_KEY"
request_timeout_seconds = 90
```

将 `ZHEN_API_KEY` 设置在启动圳的当前终端环境中。不要把真实 API Key 写入配置文件、提交到仓库或贴到公开 Issue。服务商的数据处理、可用性、价格和 API 费用由你选择的服务商决定；相关请求内容会发送给该服务商。

### Session、Workspace 与 Target

- **Session** 是一次 Agent 对话及其授权上下文。权限应按任务单独检查；撤销授权后，不应继续依赖旧授权。
- **Workspace** 是你希望 Agent 处理的项目目录或工作范围。文件访问应由用户明确授予范围。
- **Target** 是命令执行目标。Target 与本机 Host 是不同边界；只有当前构建为该平台提供并验证了执行后端时，才可使用 Target 执行。没有可用、可信的后端时，执行应保持关闭，不回退到 Host Shell。

部分 Target 设计使用 Nanos Guest、QEMU 与 SSH，将 Agent 命令送到单独的执行环境。其可用平台、运行时下载、隔离边界和验收状态会随具体 Beta 构建说明；本仓库目前没有已发布 Runtime Bundle，也不宣称所有平台均已完成隔离验收。运行前应检查将要授权的 Workspace、Target 和命令。

### Computer Use

Computer Use 是可选的实验方向，当前任务书指定的 HCU 集成目标为 macOS 与 Windows；Linux 不支持。此能力不代表已经随本仓库中的可下载构建提供，也不代表端到端验收完成。图片附件或文件上传功能尚未在此确认实现。启用视觉功能时，相关截图或上下文可能发送给你配置的模型服务商。

### Beta 限制

- 本仓库尚无公开 Release 资产；安装包、CLI、Desktop 和 Runtime Bundle 均未在此发布。
- macOS arm64、Windows x64 与 Linux x86_64 是计划打包目标，不是平台验收声明。
- Target、隔离运行时和 Computer Use 按平台及构建逐项开放；实验路径可能关闭或不完整。
- 向模型服务发送的内容、Agent 生成结果和命令效果都需要由用户审阅。重要文件请先备份。
- 不要向公开反馈渠道提交 API Key、敏感代码、私密日志或个人信息。

### 下载、反馈与许可证

- 下载：请留意[官方 GitHub Releases](https://github.com/AlbertYang514/zhen-releases/releases)。当前没有可下载资产。
- 反馈：通过[公开 Issues](https://github.com/AlbertYang514/zhen-releases/issues) 提交问题或建议。公开提交前请移除凭据、隐私数据和无权披露的内容。
- [LICENSE.txt](LICENSE.txt)：圳自有专有软件许可；版权归 Albert Yang，© 2026。
- [EULA.txt](EULA.txt)：中英双语最终用户许可协议。
- [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)：已知第三方组件、来源与待核验事项。
- [SOURCES.md](SOURCES.md)：GPL 源码及构建材料的发行状态与交付要求。
- [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md)：Beta 发布前核对清单。

圳自有软件仍按专有许可证授权。GPL/LGPL 或其他第三方许可证只适用于其覆盖的具体组件，相关权利不因圳的专有条款而改变。公开第三方源码不表示圳的 Core、CLI、Desktop、App Server 或其他自有实现开放源码。

---

## English

Zhen (Zhen Harness) is a local-first Coding + Work AI Agent Harness project. It is designed to let users work with an Agent within the model service and permissions they choose. Users bring their own model API (BYOK); Zhen does not include a model API operated by the author.

The project plans to provide a command-line interface (CLI) and a desktop client. The CLI command is `zhen`. **This repository currently has no public, verified binary release or installer. CLI downloads and desktop installers are not available yet.** Check the official releases page above; do not obtain purported installers from unofficial mirrors.

### Clients and platform status

The following are planned packaging targets for the first Beta. They are not claims that builds have shipped or passed platform acceptance:

| Client / target platform | Status |
| --- | --- |
| CLI: macOS Apple Silicon (arm64) | Planned; no public verified build |
| CLI: Windows (x64) | Planned; no public verified build |
| CLI: Linux (x86_64) | Planned; no public verified build |
| Desktop: macOS Apple Silicon / Windows x64 / Linux x86_64 | Planned clients; no downloadable installer or platform acceptance is claimed here |

### Installation and first configuration

There is no official binary available to install yet. **The configuration example below does not imply that a CLI download is available.** When a verified client is published, select the matching CLI or Desktop asset from that release. A Runtime Bundle contains execution runtime resources; it is not a Desktop installer.

The planned first-run CLI flow uses `zhen config init` to create a configuration template, with the API key stored in the environment variable named by the template. The current CLI implementation targets OpenAI-compatible Chat Completions. Once you have an official CLI build, run `zhen --help` to see the commands supported by that build.

Example configuration (placeholders only; no real credentials):

```toml
[model]
provider = "openai-compatible"
base_url = "https://your-endpoint.example/v1"
model = "your-model-id"
api_key_env = "ZHEN_API_KEY"
request_timeout_seconds = 90
```

Set `ZHEN_API_KEY` in the environment used to launch Zhen. Do not put a real API key in the config file, commit it to a repository, or post it in a public issue. The provider you choose controls data handling, availability, pricing, and API charges. Request content is sent to that provider.

### Session, Workspace, and Target

- A **Session** is an Agent conversation and its authorization context. Review permissions for each task and do not rely on stale grants after revocation.
- A **Workspace** is the project directory or work scope you want the Agent to handle. File access should be explicitly granted by the user.
- A **Target** is a command-execution destination. A Target is a separate boundary from the local Host. Target execution is available only when the specific build provides a verified backend for that platform. Without a usable, trusted backend, execution should remain disabled and must not fall back to a Host shell.

Some Target designs use a Nanos Guest, QEMU, and SSH to route Agent commands into a separate execution environment. Availability by platform, runtime downloads, isolation boundaries, and acceptance status will be documented for each Beta build. This repository currently has no published Runtime Bundle and does not claim that isolation has been accepted on every platform. Review the Workspace, Target, and commands being authorized.

### Computer Use

Computer Use is an optional experimental direction. The HCU integration targets specified for this task are macOS and Windows; Linux is unsupported. This does not mean the feature is included in a downloadable build here or has completed end-to-end acceptance. Image attachments and file uploads are not confirmed as implemented. When vision features are enabled, relevant screenshots or context may be sent to your configured model provider.

### Beta limitations

- There are no public Release assets in this repository; installers, CLI, Desktop, and Runtime Bundles have not been published here.
- macOS arm64, Windows x64, and Linux x86_64 are planned packaging targets, not platform acceptance claims.
- Target execution, runtime isolation, and Computer Use are enabled separately by platform and build; experimental paths may be unavailable or incomplete.
- Users should review model outputs and Agent command effects. Back up important files before use.
- Do not post API keys, sensitive code, private logs, or personal information in public feedback channels.

### Downloads, feedback, and license

- Downloads: watch [official GitHub Releases](https://github.com/AlbertYang514/zhen-releases/releases). No downloadable assets are available yet.
- Feedback: report issues or suggestions in [public Issues](https://github.com/AlbertYang514/zhen-releases/issues). Remove credentials, private data, and material you are not authorized to disclose before posting.
- [LICENSE.txt](LICENSE.txt): proprietary license for Zhen-owned software; Copyright © 2026 Albert Yang.
- [EULA.txt](EULA.txt): bilingual End User License Agreement.
- [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md): known third-party components, sources, and items requiring verification.
- [SOURCES.md](SOURCES.md): GPL source and build-material release status and delivery requirements.
- [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md): pre-Beta release checklist.

Zhen-owned software remains proprietary. GPL/LGPL and other third-party licenses apply only to the components they cover, and their rights are not changed by Zhen's proprietary terms. Publishing third-party source does not open-source Zhen's Core, CLI, Desktop, App Server, or other proprietary implementation.
