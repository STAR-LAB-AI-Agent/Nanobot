## 项目简介
Nanobot 是一个可扩展的 AI 助手框架，支持多种聊天应用、模型提供商和工具集成。它提供了 WebUI、CLI 和 Python SDK 等多种使用方式.
详细说明与使用方法请阅读 Nanobot 原文档：https://nanobot.wiki/cn/home/
## 快速开始
### 1. 安装
# PYPI

pip install nanobot-ai

# 源码

git clone https://github.com/HKUDS/nanobot.git

cd nanobot

pip install -e .


# 完成Quickstart：安装后必须先执行初始化

python -m nanobot onboard --wizard

安装程序会打开向导。选择 Quick Start，然后按提示完成：

1.选择拥有该凭据的 Provider 或 Endpoint。

2.按提示填写 API Key 或 Base URL。

3.填写同一个 Provider 能够运行的模型 ID。

4.允许 Quick Start 启用本地 WebUI。

5.设置 WebUI 密码，最后检查配置摘要。

### 2. 启动 WebUI
最简单的方式是直接运行：

    nanobot webui
    
该命令会自动完成配置、启用本地 WebSocket 渠道、生成启动密钥、启动网关并打开浏览器。默认绑定 127.0.0.1，仅本机可访问。

### 3. 发送第一条消息
打开 Settings → Models，配置 Provider、凭据和当前模型 Preset。
新建 Topic 并发送 Hello!，先确认所选模型可以正常回复。

### 如何在nanobot集成调用skill

1. 初始化完成后生成默认目录如下：
- Windows：`C:\Users\用户名\.nanobot\workspace`
- Linux/macOS：`~/.nanobot/workspace`

2. 把整个 skill 文件夹放入：
  `~/.nanobot/workspace/skills/`

其中标准的skill文件结构应为

<skill‑name>/

├── SKILL.md 

├── scripts/

│        └── main.py    

├── requirements.txt  

└── README.md             

4. 重启 Nanobot，通过自然语言调用skill。

### WebUI 主要功能区域

| 区域 | 用途 |
|------|------|
| Topics | 新建、切换、搜索、分叉和删除对话 Topic |
| Agent activity | 查看思考过程、工具调用、文件差异和生成结果 |
| Workspace | 为 Agent 选择项目工作区（影响文件操作和 Shell 命令） |
| Access | 选择当前 Topic 的本地能力访问模式 |
| Composer | 发送文字、图片、文档、语音、斜杠命令，或 @ 提及 App/MCP |
| Channels | 连接 Telegram、Discord、Slack、飞书、微信等聊天平台 |
| Apps | 安装、测试和管理本地 CLI App 适配器与 MCP Preset |
| Skills | 查看内置和工作区提供的技能指令 |
| Automations | 管理定时任务和本地触发器 |
| Settings | 调整模型、Provider、图像、语音、网页工具、运行时和安全选项 |

### Topic 与工作区

- 每个 Topic 拥有独立的历史、标题、工作区和自动化关联。
- 需要独立上下文时新建 Topic；想从已有对话分支继续时使用 Fork。
- 处理具体项目前先选择工作区，以便 Agent 建立正确的项目上下文。
- 输入框旁的访问控制决定当前聊天可使用的本地能力：
  - Restricted 模式：文件和 Shell 操作限制在所选项目中，但保留对内置 Skills、Agent 工作区自定义 Skills 和精确记忆文件的只读访问。
  - Full Access：允许更大范围的本地操作（需 Gateway 允许）。

### 输入框使用

- 支持普通文字、图片、文档附件（PDF、DOCX、XLSX、PPTX、TXT、Markdown、CSV、JSON、XML、HTML、YAML、TOML、日志等）、语音输入（需配置转录服务）、斜杠命令。
- 通过 @ 提及已安装的 App 或 MCP Preset，可将工具附加到当前消息。
- 模型徽标显示当前模型或 Preset；未配置时会链接到模型设置。
- 如需生成图片，先配置图像 Provider，然后在输入框切换到图像模式

### 聊天渠道

- 打开 Settings → Channels，无需手写 JSON 即可连接聊天应用。
- 引导式设置支持：安装可选渠道包、收集凭据、处理二维码登录、验证连接、提示何时重启网关。
- 新渠道建议先用私人消息测试；使用配对码机制时，在 WebUI 中批准发送者。
- 除非明确需要公开访问，否则不要使用通配符 Allowlist。

### Apps 与 MCP 集成

- Apps 是本地命令行适配器，由 Nanobot 在电脑上运行；安装适配器不会修改原生应用或网站。
- Integrations 是 MCP 服务器，Preset 提供已知配置，自定义集成支持 stdio、HTTP 和 SSE。
- 部分 MCP Preset（如 Firecrawl、Parallel Search）可连接无需 API Key 的托管 Endpoint，但不会替代内置网页搜索 Provider。
- 应用或集成可用后，在输入框用 @ 提及即可附加到对话。

### Skills

- Skills 页面显示当前可用的技能指令，包括内置和来自工作区的 Skill。
- 在交给 Agent 复杂任务前，可先查看是否已有对应流程。

### 自动化任务

- 自动化任务是与 Topic 关联的未来对话轮次，应在目标 Topic 或渠道中创建。
- 定时任务：由 Cron 工具创建，支持指定时间、固定间隔或 Cron 表达式。
- 本地 Trigger：通过 /trigger <name> 创建，然后使用本地命令触发，例如：

      nanobot trigger trg_8K4P2Q9X "Review PR #4502"

- Automations 页面支持筛选、搜索、排序、立即运行、暂停/恢复、重命名、删除，以及复制 Trigger 命令。
- 没有关联 Topic 的自动化任务无法从 WebUI 启用或运行。

### 设置

- Settings 是浏览器会话和网关运行时配置的控制中心。
- Provider 和模型 Preset 的修改在下一个 Turn 生效，无需重启网关。
- 修改渠道进程、网络绑定、可选包等可能需要重启，WebUI 会显示提示。
- 仅影响浏览器的偏好（如文件编辑显示模式）立即生效。

### 启用网页搜索

在本机通过 WebUI 设置：

运行 nanobot webui。
打开 Settings → Web。
启用网页搜索，选择 Provider；如果页面要求，填写 API Key。
保存；出现提示时重启。
提出一个必须查询最新资料才能回答的问题，并检查回复引用的来源。
如果配置由文件或部署系统管理，可以显式使用默认搜索 Provider：

{
  "tools": {
    "web": {
      "enable": true,
      "search": {
        "provider": "duckduckgo"
      }
    }
  }
}

也可以使用需要 API 的 Provider：

{
  "tools": {
    "web": {
      "search": {
        "provider": "brave",
        "apiKey": "${BRAVE_API_KEY}"
      }
    }
  }
}

配置后，提出一个需要最新信息的问题，再从 WebUI 或日志中查看工具执行过程。

### 图像生成

快速设置
通过 WebUI 设置

1.如果尚未配置图像 Provider 的凭据，先到设置 → 模型中添加。
2.打开设置 → 图像。
3.选择 Provider 和图像模型，然后启用图像生成。
4.保存后，让 nanobot 生成一张简单图片进行测试。如果 Gateway 无法实时应用该修改，WebUI 会提示你重启。

在 WebUI 中使用
1.打开设置，选好已经配置的 Provider 和模型，并启用图像生成。
2.在聊天中描述你想生成或修改的图片。
3.如果默认尺寸不合适，请在要求中明确写出宽高比或大小。
4.编辑现有图片时，请附上参考图。

# 添加MCP工具
最小可用示例
在本机交互式配置时：

运行 nanobot webui，打开 Apps。
选择一个已有的集成 Preset，或者添加自定义 stdio、HTTP 或 SSE 服务器。
如果服务器提供的工具超过当前任务需要，只启用必要工具。
保存；页面提示需要重启时，按提示重启。
在下一条消息中用 @ 提及该集成，并让 Agent 完成一个很小的测试操作。
