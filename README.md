# openclaw-installer
可以根据命令行意见部署openclaw
我来为你提供 OpenClaw Installer 的详细使用教程。

---

## OpenClaw Installer 使用教程


### 快速开始

#### 1. 进入项目目录
```bash
cd c:\Users\你的实际目录openclaw-installer
```

#### 2. 查看帮助信息
```bash
node dist/index.js --help
```

输出：
```
Usage: openclaw-install [options] [command]

Options:
  -V, --version        output the version number
  -s, --silent         Silent mode (non-interactive) (default: false)
  -q, --quick          Quick install with defaults (default: false)
  -r, --repair         Repair existing installation (default: false)
  -c, --config <path>  Path to config file
  --model <provider>   AI model provider (kimi|openai|anthropic|ollama)
  --api-key <key>      API key for the model provider
  --port <port>        Gateway port
  --registry <url>     npm registry URL
  -v, --verbose        Verbose output (default: false)
  -h, --help           display help for command

Commands:
  uninstall [options]  Uninstall OpenClaw
  status               Check installation status
```

---

### 常用命令详解

#### 1. 检查环境状态（推荐首先运行）

```bash
node dist/index.js status
```

**功能**：检测系统环境，包括：
- Node.js 版本和路径
- npm 版本和路径
- OpenClaw 安装状态
- 端口占用情况（18789, 19001-19003）

**示例输出**：
```
◇  检测完成

  环境信息:
  ├─ Node.js: 24.14.0
  ├─ npm: 11.9.0
  └─ 平台: windows x64

  OpenClaw:
  ├─ 已安装: 2026.2.26
  └─ 路径: C:\Users\80468\AppData\Roaming\npm\openclaw

  端口状态:
  ├─ 18789: 被占用 (svchost.exe)
  ├─ 19001: 被占用 (node.exe)
  ├─ 19002: 可用
  └─ 19003: 可用
```

---

#### 2. 交互式安装（推荐）

```bash
node dist/index.js
```

**安装流程**：

1. **环境检测** - 自动检测 Node.js、npm、端口等
2. **选择 AI 模型提供商**：
   - Kimi (月之暗面) - 推荐国内用户
   - OpenAI (GPT-4)
   - Anthropic (Claude)
   - Ollama (本地)
   - 跳过，稍后配置
3. **输入 API Key** - 根据选择的模型输入对应的 API Key
4. **设置 Gateway 端口** - 默认自动选择可用端口
5. **自动安装** - 下载并配置 OpenClaw
6. **启动 Gateway** - 自动启动服务
7. **完成** - 显示访问地址

---

#### 3. 快速安装（使用默认配置）

```bash
node dist/index.js --quick
```

**特点**：
- 非交互式，一键完成
- 使用默认配置（Kimi 模型，端口 19001）
- 适合熟悉配置的用户

---

#### 4. 静默安装（自动化部署）

```bash
node dist/index.js --silent --model kimi --api-key "your-api-key" --port 19002
```

**参数说明**：
- `--silent`：静默模式，无交互
- `--model`：模型提供商 (kimi/openai/anthropic/ollama)
- `--api-key`：API 密钥
- `--port`：Gateway 端口
- `--registry`：指定 npm 镜像源

---

#### 5. 修复安装

```bash
node dist/index.js --repair
```

**适用场景**：
- OpenClaw 已安装但配置损坏
- 需要重新初始化配置
- Gateway 无法正常启动

---

#### 6. 卸载 OpenClaw

```bash
# 交互式卸载（有确认提示）
node dist/index.js uninstall

# 强制卸载（无确认）
node dist/index.js uninstall --yes
```

---

### 高级用法

#### 1. 使用指定配置文件

```bash
node dist/index.js --config ./my-config.json
```

配置文件格式：
```json
{
  "meta": {
    "lastTouchedVersion": "2026.2.26",
    "lastTouchedAt": "2026-02-28T12:00:00.000Z"
  },
  "agents": {
    "defaults": {
      "workspace": "C:\\Users\\{用户名}\\.openclaw\\workspace",
      "model": "moonshot/kimi-k2.5"
    }
  },
  "gateway": {
    "mode": "local",
    "port": 19001,
    "auth": {
      "token": "auto"
    }
  }
}
```

---

#### 2. 使用国内镜像源

```bash
node dist/index.js --registry https://registry.npmmirror.com
```

---

#### 3. 详细日志输出

```bash
node dist/index.js --verbose
```

日志文件位置：
```
C:\Users\{用户名}\.openclaw-installer\logs\install.log
```

---

### 安装后的使用

安装完成后，你可以：

#### 1. 访问 Dashboard
```
http://127.0.0.1:{端口}/
```
例如：`http://127.0.0.1:19001/`

#### 2. 启动 TUI 界面
```bash
openclaw tui
```

#### 3. 查看 OpenClaw 帮助
```bash
openclaw --help
```

#### 4. 配置模型
```bash
openclaw config models
```

#### 5. 检查健康状态
```bash
openclaw health
```

---

### 六、常见问题

#### Q1: 端口被占用怎么办？
安装程序会自动检测端口占用情况，并选择可用端口（如 19002, 19003）。你也可以手动指定：
```bash
node dist/index.js --port 19002
```

#### Q2: 安装过程中断怎么办？
重新运行安装命令，安装程序会自动恢复：
```bash
node dist/index.js --repair
```

#### Q3: 如何更换 AI 模型？
```bash
openclaw config models
```
或在 Dashboard 中配置。

#### Q4: 安装失败如何排查？
1. 查看日志：`C:\Users\{用户名}\.openclaw-installer\logs\install.log`
2. 使用详细模式：`node dist/index.js --verbose`
3. 检查网络连接和 npm 镜像源

---

### 七、完整使用示例

**场景 1：全新安装（推荐）**
```bash
cd c:\Users\80468\Documents\trae_projects\123asd\openclaw-installer
node dist/index.js status          # 先检查环境
node dist/index.js                  # 开始交互式安装
```

**场景 2：自动化部署**
```bash
node dist/index.js --silent --model kimi --api-key "sk-xxx" --port 19002 --registry https://registry.npmmirror.com
```

**场景 3：修复并重装**
```bash
node dist/index.js uninstall --yes  # 先卸载
node dist/index.js --quick          # 快速重装
```

---

你想试用哪个功能？我可以帮你运行具体的命令。
