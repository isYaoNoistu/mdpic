# Hermes Agent 

## 一、Hermes Agent 介绍

### 1、Hermes 简介

官方是这样介绍的：

> 部署在你的服务器上，连接你的消息账号，它就成为你的持久个人智能体——学习你的项目、自动构建技能、随时随地触达你。不是聊天机器人，不是代码补全工具，而是一个住在你机器上、每天都在变聪明的智能体。
>
> - GitHub 仓库：https://github.com/nousresearch/hermes-agent
> - Hermes Agent 官网：https://hermes-agent.nousresearch.com/

![image-20260421164821850](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421164821850.png)

事实上，Hermes Agent 的核心理念是：**让 AI 成为长期在线的数字员工，而非一次性聊天机器人，**是业内少见的**原生内置学习闭环的 AI Agent**，可从执行经验中沉淀技能、自主优化能力、持久化知识、检索历史对话，并在跨会话中持续完善用户认知模型。





## 二、Hermes Agent 安装

安装方式提供两种可选：

- 采用官方脚本安装

- 使用github源码进行二进制安装

### 1、官方脚本安装 WSL系统

#### 1.1 环境准备

网络环境需要进行处理，毕竟hermes的代码在Github上，所以需要科学上网，win主机开启系统代理：

![image-20260421162154161](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421162154161.png)

开启系统代理后，由于我们是WSL启动的虚拟机，需要直接代理wsl服务器的网关，在win主机上查看：

![image-20260421162332914](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421162332914.png)

在wsl虚拟机上编写环境变量：

```bash
vim /etc/profile
# 网络代理
proxy () {
  export http_proxy="http://172.19.128.1:7890"
  export https_proxy=$http_proxy
  export socks5_proxy="socks5://172.19.128.1:7890"
  echo "HTTP Proxy on"
}

# noproxy
noproxy () {
  unset http_proxy
  unset https_proxy
  echo "HTTP Proxy off"
}
```

相当于增加两个命令，`proxy`命令用于开启代理，`noproxy`用于关闭代理。

```bash
source /etc/profile

# 开启代理
proxy
HTTP Proxy on
```

> 使用VMware启动的虚拟机依旧可以使用该方式进行添加网络代理，使用nat模式，网络代理nat网关即可。
>
> 不再做相关编写，方式一样，本文也适配Linux系统，可直接复用。

#### 1.2 使用官方脚本安装

使用wsl系统进行安装：

> 官方提醒：原生 Windows 支持仍处于实验阶段，请安装 WSL2 后在其中运行 Hermes Agent。
>
> 个人也不建议把OpenClaw这一类产品安装在windows上。

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

![image-20260421164445121](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421164445121.png)

安装程序会自动处理所有依赖项：

- **uv** — 快速 Python 包管理器
- **Python 3.11** — 通过 uv 安装，无需 sudo
- **Node.js v22** — 用于浏览器自动化和 WhatsApp 桥接
- **ripgrep** — 快速文件搜索
- **ffmpeg** — TTS 音频格式转换

> 也可手动补全相关依赖文件：

```bash
sudo apt update && sudo apt install python3.11 python3.11-venv
```

安装完成后，进行初始化配置。

#### 1.3 初始化模式选择

**Quick setup（快速设置）**：只配置**核心必需项**：AI 服务商、模型、消息功能，完全满足日常使用，不用折腾复杂设置

**Full setup（完整设置）**：配置**所有高级功能**：网络端口、日志、插件、权限等

我们选择第一项进行快速设置：

![image-20260421170809701](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421170809701.png)

#### 1.4 模型供应商与KEY填写 

选择模型供应商，根据自身需求和情况进行选择。

![image-20260421170910695](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421170910695.png)

我选择使用自定义供应商：

![image-20260421171251112](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171251112.png)

**填写供应商API：**

![image-20260421171358500](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171358500.png)

填写完供应商API后，填写密钥，密钥默认不显示在终端，点击回车后**选择模型配置，会根据所填密钥进行分析和输出可用的大模型：**

![image-20260421171427182](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171427182.png)

#### 1.5 模型初始配置

**留空进行自动检测**，系统会自动用模型的最大默认上下文长度。

> Context length in tokens [leave blank for auto-detect]

![image-20260421171532982](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171532982.png)

选择是否对接外部平台：我们暂不选择，**选择第二项**进行跳过，后续进行设置。

![image-20260421171715727](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171715727.png)

见到以下界面代表配置完成：

![image-20260421171814560](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171814560.png)

开启聊天：

> Launch hermes chat now? [Y/n]: Y

![image-20260421172530407](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421172530407.png)

测试使用：

![image-20260421172518274](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421172518274.png)



### 2、手动安装 Ubuntu22.04

#### 2.1 克隆仓库

使用 `--recurse-submodules` 克隆以获取所需的子模块：

```bash
mkdir /data
cd /data
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent/
```

如果已经克隆但没有子模块，使用下述命令补齐：

```bash
git submodule update --init --recursive
```

#### 2.2 安装 uv 并创建虚拟环境

安装uv：

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env 

# 测试 uv
uv --version 
uv 0.11.7 (x86_64-unknown-linux-gnu)
```

创建 Python 3.11 的虚拟环境:

```bash
pwd 
/data/hermes-agent

# 创建虚拟环境
uv venv venv --python 3.11
```

#### 2.3 安装 Python 依赖

```bash
# 告诉 uv 要安装到哪个虚拟环境
export VIRTUAL_ENV="$(pwd)/venv"

# 安装所有扩展
uv pip install -e ".[all]"
```

#### 2.4 创建配置目录

```bash
# 创建目录结构
mkdir -p ~/.hermes/{cron,sessions,logs,memories,skills,pairing,hooks,image_cache,audio_cache,whatsapp/session}

# 复制示例配置文件
cp cli-config.yaml.example ~/.hermes/config.yaml

# 创建空的 .env 文件用于存储 API 密钥
touch ~/.hermes/.env
```

#### 2.5 添加环境变量

将 hermes 添加到 PATH

```bash
mkdir -p ~/.local/bin
ln -sf "$(pwd)/venv/bin/hermes" ~/.local/bin/hermes

# Bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```

#### 2.6 配置模型

```bash
# 执行命令
hermes model

# 在此处添加模型供应商
API base URL [e.g. https://api.example.com/v1]: https://api.edgefn.net/v1

# 在此处添加密钥
API key [sk-sWHHu...]: 
Verified endpoint via https://api.edgefn.net/v1/models (27 model(s) visible)
  Available models:
    1. DeepSeek-V3.2
    2. Kimi-K2-Instruct
    3. KAT-Coder-Exp-72B-1010
    4. Qwen3-Next-80B-A3B-Instruct
    5. Qwen3-Next-80B-A3B-Thinking
    6. Qwen3-Coder-480B-A35B-Instruct
    7. Qwen2.5-72B-Instruct
    8. Qwen3-235B-A22B
    9. DeepSeek-R1-0528
    10. DeepSeek-R1-Distill-Qwen-32B
    11. GLM-5
    12. bge-reranker-v2-m3
    13. Qwen3-30B-A3B-FP8
    14. GLM-4.6
    15. GLM-4.5V
    16. BAAI/bge-m3
    17. DeepSeek-V3.2-EXP
    18. DeepSeek-V3
    19. GLM-4.7
    20. KAT-Coder
    21. Qwen3-235B-A22B-2507
    22. Qwen3-32B-FP8
    23. DeepSeek-R1-0528-Qwen3-8B
    24. DeepSeek-R1-Distill-Qwen-14B
    25. KAT-Coder-Pro-V1
    26. GLM-4.5
    27. MiniMax-M2.5
  
  # 在此处选择可用的大模型ID
  Select model [1-27] or type name: 11

# 大模型参数配置
Context length in tokens [leave blank for auto-detect]: 

# 模型供应商名字
Display name [Api.edgefn.net]: 
Default model set to: GLM-5 (via https://api.edgefn.net/v1)
  💾 Saved to custom providers as "Api.edgefn.net" (edit in config.yaml)
```

> 附带图例：

选择模型供应商，根据自身需求和情况进行选择。

![image-20260421170910695](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421170910695.png)

我选择使用自定义供应商：

![image-20260421171251112](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171251112.png)

**填写供应商API：**

![image-20260421171358500](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171358500.png)

填写完供应商API后，填写密钥，密钥默认不显示在终端，点击回车后**选择模型配置，会根据所填密钥进行分析和输出可用的大模型：**

![image-20260421171427182](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421171427182.png)

#### 2.7 测试服务

开启服务：

```bash
hermes
```

![image-20260421215135528](https://raw.githubusercontent.com/isYaoNoistu/mdpic/main/2026/04/image-20260421215135528.png)

### 3、服务检测





## 三、Hermes 基本使用

### 1、hermes 命令

Hermes Agent 的 CLI 是一个完整的终端用户界面（TUI），而非网页端。Hermes Agent CLI 支持多行编辑、斜杠命令自动补全、对话历史、中断重定向以及流式工具输出，专为习惯在终端中工作的用户设计。

运行CLI命令汇总：

```bash
hermes              # 交互式命令行界面 — 开启对话
hermes model        # 选择大语言模型服务商与对应模型
hermes tools        # 配置启用的工具集
hermes config set   # 设置单项配置项
hermes gateway      # 启动消息网关（支持Telegram、Discord等平台）
hermes setup        # 运行全量配置向导（一站式完成所有配置）
hermes claw migrate # 从OpenClaw迁移数据（适用于原OpenClaw用户）
hermes update       # 更新至最新版本
hermes doctor       # 诊断运行环境与配置问题
```

常用命令组合：

```bash
# 启动交互式会话（默认）
hermes

# 单次查询模式（非交互式）
hermes chat -q "Hello"

# 指定模型
hermes chat --model "anthropic/claude-sonnet-4"

# 指定提供商
hermes chat --provider openrouter  # 强制使用 OpenRouter

# 指定工具集
hermes chat --toolsets "web,terminal,skills"

# 启动时预加载一个或多个技能
hermes -s hermes-agent-dev,github-auth
hermes chat -s github-pr-workflow -q "open a draft PR"

# 恢复之前的会话
hermes --continue                   # 恢复最近的 CLI 会话 (-c)
hermes --resume &lt;session_id&gt;  # 通过 ID 恢复指定会话 (-r)

# 详细模式（调试输出）
hermes chat --verbose

# 隔离的 git worktree
hermes -w                         # 在 worktree 中交互式运行
hermes -w -q "Fix issue #123"     # 在 worktree 中单次查询
```

### 2、`/` 命令

Hermes 支持大量 CLI 斜杠命令、动态技能命令以及用户定义的快速命令。

| 命令                   | 说明                          |
| :--------------------- | :---------------------------- |
| `/help`                | 显示命令帮助                  |
| `/model`               | 显示或更改当前模型            |
| `/tools`               | 列出当前可用的工具            |
| `/skills browse`       | 浏览技能中心及官方可选技能    |
| `/background <prompt>` | 在独立的后台会话中运行 prompt |
| `/skin`                | 显示或切换活动的 CLI 皮肤     |
| `/voice on`            | 启用 CLI 语音模式             |
| `/voice tts`           | 切换 Hermes 回复的语音播放    |
| `/reasoning high`      | 增加推理深度                  |
| `/title My Session`    | 为当前会话命名                |

