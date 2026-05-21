# Hermes Desktop

[English](README.md) | [简体中文](README.zh-CN.md)

Hermes Desktop 是 Hermes Agent 的原生 macOS 伴侣应用，通过 SSH 连接你的 Hermes 主机。

它把日常 Hermes 工作流变成一个可以长期停留的 Mac 桌面工作区：会话、规范文件、用量、技能、定时任务和真实终端都集中在一个清晰窗口里。

如果 Hermes 已经是你的工作方式，Hermes Desktop 应该会很容易理解：同一台主机、同一批文件、同一个 shell、同一套 profile、同一个调度器、同一份会话历史。

它不是浏览器外壳，不依赖网关 API，不在远程主机安装守护进程，不把文件镜像到本机，也不引入一层会慢慢偏离真实主机的同步状态。

这种克制是刻意设计的：

- 直接通过 SSH 连接
- 保持 Hermes 主机作为唯一事实来源
- 不依赖 gateway API
- 不把文件镜像到 Mac
- 不在远程主机安装 helper service

Hermes Desktop 不会发明一个更温和的第二版 Hermes。它让真实 Hermes 工作流在 Mac 上变得安静、快速、原生，同时保持底层模型可见。你仍然知道自己在哪台主机上、哪个 Hermes profile 正在生效、规范状态在哪里、应用实际使用的是哪条路径。

## 预览

<table>
  <tr>
    <td width="50%">
      <img src="assets/CRON-JOBS.png" alt="Hermes Desktop Cron Jobs view" />
    </td>
    <td width="50%">
      <img src="assets/USAGE.png" alt="Hermes Desktop Usage view" />
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="assets/SKILLS.png" alt="Hermes Desktop Skills view" />
    </td>
    <td width="50%">
      <img src="assets/TERMINALE.png" alt="Hermes Desktop Terminal view" />
    </td>
  </tr>
</table>

上图展示的是连接真实 Hermes 主机后的 Cron Jobs、Usage、Skills 和 Terminal 页面，并已为公开 README 做隐私处理。

## 你会得到什么

- 一个真正像 Mac 应用的原生桌面应用，而不是浏览器控制台
- 面向默认 Hermes home 和同一主机上命名 Hermes profile 的直接 SSH 连接配置
- profile 感知工作区：Overview、Files、Sessions、Usage、Cron Jobs、Skills 和 Terminal 都会基于当前选择的 Hermes profile 解析
- 内嵌真实 SSH 终端，支持跨主机和 profile 的多标签页、快速主题、实时背景色和文字颜色调整
- 自然的 macOS 多 Agent 工作流：一个标签页跑 shell，一个看调度器，一个切到不同 profile，不需要发明第二套主机模型
- Overview 展示当前 profile、已发现 profiles、解析路径、session store、cron 位置和主机就绪检查
- 带冲突检测的规范 Hermes 文件编辑：
  - `~/.hermes/memories/USER.md`
  - `~/.hermes/memories/MEMORY.md`
  - `~/.hermes/SOUL.md`
- 从远程规范 session store `~/.hermes/state.db` 浏览、搜索和删除会话
- 仅当 SQLite session store 不可用时，才回退到 `~/.hermes/sessions/*.jsonl`
- 汇总用量、近期趋势、模型拆分，以及可读多个 Hermes profile 时的主机级跨 profile 总计
- 从 `~/.hermes/skills/**/SKILL.md` 递归浏览技能
- 在 Hermes Desktop 中直接编辑和创建技能，保存时使用原子写入，并检查远程 `SKILL.md` 冲突
- 浏览、创建、编辑、暂停、恢复、立即运行和删除远程规范调度器状态 `~/.hermes/cron/jobs.json` 中的 cron job
- 同一构建流程生成适用于 Apple Silicon 和 Intel Mac 的 universal 包

只要 Hermes 在那里运行，并且 SSH 已经可用，Hermes Desktop 通常就可以连接。典型目标包括：

- Raspberry Pi
- 另一台 Mac
- VPS 或远程服务器
- 同一台 Mac，通过 `ssh localhost`、本地主机名或本地 SSH alias

## Hermes Desktop 与官方 Web Dashboard

Nous Research 现在提供官方 Hermes web dashboard，这是好事，因为它让产品边界更清晰。

官方 dashboard 适合浏览器管理场景。Hermes Desktop 适合你希望在 Mac 上长期停留的那部分 Hermes 工作流。

分工很简单：

- 官方 web dashboard：用于浏览器中的管理任务，例如配置、API keys、日志和 dashboard 式管理
- Hermes Desktop：用于希望主机本身在 macOS 上变得原生时的工作流，例如直接 SSH、规范文件、真实会话、profile 感知用量、cron 工作流、可编辑技能和真实终端

Hermes Desktop 不想把 Hermes 拖进一个介于浏览器 UI 和主机现实之间的模糊中间层。它面向那些希望靠近主机、通过真实 SSH 路径工作，同时拥有精致原生 Mac 工作区的人。

## 下载前准备

准备工作刻意保持轻量。你只需要：

- 一台运行 macOS 14 或更新版本的 Mac
- 这台 Mac 已经可以在 Terminal 中无交互提示地 SSH 到目标主机
- 目标主机的 SSH host key 已经在 Terminal 中接受过一次
- 这台 Mac 到 Hermes 主机有正常网络路径，例如本地局域网、公网 IP/DNS、VPN、Tailscale IP 或 hostname
- Hermes 主机上可用 `python3`
- Hermes 数据位于远程用户的 `~/.hermes`

简单规则：如果下面命令可以在这台 Mac 的 Terminal 中运行，并且不会要求输入密码或确认 host key，那么应用通常也能工作：

```bash
ssh your-host
```

## 安装

安装大约一分钟：

1. 从 GitHub Releases 下载 `HermesDesktop.app.zip`。
2. 双击 zip 解压。
3. 将 `HermesDesktop.app` 拖到 `Applications`。
4. 打开应用。

公开发布包是同时支持 Intel 和 Apple Silicon Mac 的 universal macOS build。目前尚未 notarize，所以 macOS 可能会提示 Apple 无法验证该应用是否包含恶意软件。这是当前发布流程的预期限制，并不表示 macOS 发现了恶意软件。

如果 macOS 阻止首次启动：

1. 点击 `Done`，不要点 `Move to Bin`。
2. 右键点击 `HermesDesktop.app`，选择 `Open`。
3. 如有需要，进入 `Privacy & Security` 并点击 `Open Anyway`。

## 连接 Hermes 主机

打开应用，进入 `Connections`，创建 profile，然后点击 `Test` 和 `Use Host`。

填写连接有两种有效方式。多数情况下，SSH alias 是最干净的方式。

### 方式 1：SSH alias

SSH alias 是保存在 Mac SSH 配置中的短名称。你可以用一个简单名称代替完整 SSH 命令，例如：

```bash
ssh hermes-home
```

它通常来自 `~/.ssh/config`。

示例：

```sshconfig
Host hermes-home
  HostName vps.example.com
  User alex
```

在应用中：

- 将 `SSH alias` 设为 `hermes-home`
- `Host`、`User` 和 `Port` 留空，除非你想显式覆盖

### 方式 2：直接填写主机详情

如果你平时这样连接：

```bash
ssh alex@vps.example.com
```

那么在应用中填写：

- `Host or IP`: `vps.example.com`
- `User`: `alex`
- `Port`: `22` 或你的真实 SSH 端口

### 同一主机上的 Hermes profiles

Hermes Desktop 可以指向同一 SSH 主机上的默认 Hermes home，也可以指向命名 profile。

示例：

- `Hermes profile` 留空表示使用 `~/.hermes`
- `Hermes profile` 填 `researcher` 表示使用 `~/.hermes/profiles/researcher`

关键在于：profile 选择不是表单上的标签，而会贯穿整个应用。Overview、Usage、Cron Jobs 和 Terminal 都会根据该 profile 解析；终端会带着正确的 `HERMES_HOME` 启动；终端标签页也可以跨不同 profile 保持打开。

### 同一台 Mac

如果 Hermes 运行在同一台 Mac 上，模型仍然保持一致：SSH。

可以使用：

- `localhost`
- 本地主机名
- 本地 SSH alias

Hermes Desktop 仍然通过 SSH 连接，不会直接读取本地文件。

## `Test` 会检查什么

`Test` 是预检，不是装饰按钮。

它会检查：

- SSH 目标可达
- 认证可以无交互提示完成
- 应用使用的远程 SSH 环境中可用 `python3`

如果 `Test` 通过，`Use Host` 通常就有可靠基础。

## 应用里能看到什么

- `Overview`：确认当前主机、当前 Hermes profile、已发现 profiles、跟踪路径、cron 位置和 session store 来源。
- `Files`：编辑主机上的 `USER.md`、`MEMORY.md` 和 `SOUL.md`，保存前进行远程冲突检查。
- `Sessions`：从真实远程 session store `~/.hermes/state.db` 读取会话，支持搜索、更清晰的元数据、进入页面刷新和远程删除。
- `Cron Jobs`：浏览真实 Hermes cron 定义，支持创建、编辑、暂停、恢复、立即运行、删除，并显示 schedule、model、skills、delivery target 和近期状态。
- `Usage`：显示输入/输出 token 汇总、热门 sessions、热门 models、近期 session 趋势，以及可用时的主机级 profile 拆分。
- `Skills`：发现、读取、创建和编辑 `~/.hermes/skills/` 下的远程 `SKILL.md` 文件，支持快速过滤、伴随文件夹感知、可选目录脚手架和保存前远程冲突检查。
- `Terminal`：在应用内打开真实 SSH shell，支持多标签页、快速主题预设、实时颜色调整，以及贴近主机的多 profile、多 Agent 工作流。

## 为什么它感觉不同

Hermes Desktop 来自真正使用 Hermes 后对细节的在意：

- 选中的 Hermes profile 不是装饰，它在整个应用中保持一致
- 终端标签页不是摆设，它让你跨主机和 profile 保持并行 Agent 工作
- Sessions 和 Usage 来自规范远程存储，而不是本地第二套解释
- Memories 和 Skills 的编辑会原子保存并尊重远程状态，而不是盲目覆盖
- Cron 工作流和主机工作流放在一起，而不是作为另一个孤立产品

结果是一个安静的 Mac 应用。它不是靠隐藏底层系统获得安静，而是靠始终贴近真实主机。

## 为什么使用 SSH 和真实终端

Hermes 最强的地方在命令行。

Hermes Desktop 尊重这一点：真实 SSH、真实终端、真实远程文件、真实 session 数据、真实 cron 状态。

它不试图用独立网关层隐藏 Hermes，不发明第二个事实来源，也不把工作流变成更软但更不可靠的东西。目标不是抽象掉 Hermes，而是给 Hermes 一个诚实的原生 Mac 表面。

## FAQ

### 安装安全吗？

这是正确的问题，不应该只依赖口头保证。

你可以自己验证：

- 应用在此仓库开源，你可以用 `./scripts/build-macos-app.sh` 本地构建，而不是使用 release zip
- GitHub release 会显示发布产物的 SHA-256，你可以下载后用 `shasum -a 256 HermesDesktop.app.zip` 对比
- 你可以本地验证 app bundle：`codesign --verify --deep --strict /Applications/HermesDesktop.app`
- Hermes Desktop 使用你选择的主机的直接 SSH，不需要 gateway API；如需检查运行时网络行为，可以用 Little Snitch、LuLu 或 `nettop`
- Hermes Desktop 不需要在远程主机安装 helper service；如果谨慎，可以先在一次性或非关键 Hermes 主机上测试
- 如果你已经信任某个 coding agent，可以让它审查此仓库、构建脚本、打包流程和发布流程

当前重要限制是分发信任：公开 build 是同时支持 Intel 和 Apple Silicon 的 universal 包，但尚未经过 Apple notarization。因此 macOS 可能会显示首次启动警告。

### 有官方 Web Dashboard，为什么还需要 Hermes Desktop？

因为它们解决不同问题。

官方 dashboard 是浏览器管理界面。Hermes Desktop 是直接基于 SSH 的原生 Mac 日常工作区。配置、API keys、日志等浏览器管理流程适合 dashboard；sessions、规范文件、cron jobs、profile 感知 usage、可编辑 skills 和真实 terminal 适合 Hermes Desktop。

### 为什么不能浏览 Agent 在主机上创建的所有文件？

这是有意设计。Hermes Desktop 不想变成远程文件管理器或完整远程 IDE。它保持聚焦：sessions、memories、cron 和 terminal。

如果你需要完整文件系统访问，已有更合适的工具：普通 SSH shell、SFTP 应用或远程编辑器。限制应用内文件表面也避免鼓励用户随意打开未经审查的任意 Agent 生成文件。

### 为什么仍然需要 Terminal 里 SSH 先可用？

因为应用不会替代 SSH。它使用你的 Mac 已经可用的同一条连接路径，但以非交互方式运行。

如果 Terminal 仍然需要输入密码、确认 host key 或处理其他交互问题，应用通常也会遇到同样的阻碍。

### Mac 必须和 Hermes 主机在同一个 Wi-Fi 或局域网吗？

不需要。

Mac 只需要能通过普通 SSH 路由访问主机。可以是：

- 同一局域网
- 公网 IP 或 DNS
- VPN
- Tailscale IP 或 MagicDNS hostname

如果 `ssh your-host` 从这台 Mac 可用，Hermes Desktop 通常可以使用同一路径。

一个重要细节：Hermes Desktop 使用标准 `/usr/bin/ssh`。如果你的设置只能通过单独的 `tailscale ssh` 命令工作，而不能通过普通 `ssh` 工作，那么应用内行为可能不同。

### 为什么不把 Hermes 文件镜像到 Mac？

因为远程 Hermes 主机应保持事实来源。一旦应用开始本地缓存或同步副本，就会引入过期状态、冲突处理和更难解释的行为。当前设计让读取和编辑始终附着在真实远程文件上。

### 为什么优先从 `~/.hermes/state.db` 读取 sessions？

因为这是规范 Hermes session store。读取它能让应用获得 Hermes 自身使用的同一视图。`~/.hermes/sessions/*.jsonl` 只在 SQLite store 不可用时作为回退。

### 如果远程文件在我打开后发生变化怎么办？

Hermes Desktop 不会盲目覆盖。

保存 `USER.md`、`MEMORY.md` 或 `SOUL.md` 前，应用会检查远程文件是否仍然匹配你最初加载的版本。如果主机上的文件已变化，保存会被阻止，你的本地编辑会保留。此时应用会要求先 `Reload from Remote`，让你有意识地处理冲突，而不是静默覆盖较新的远程状态。

## 路线图

原始路线图中的大部分内容已经交付。

当前应用已经达到目标：一个安静、强大的原生 macOS 工作区，服务真实 Hermes 工作流，并继续以 SSH 和主机事实来源为核心。

### 已交付

- [x] 围绕规范 Hermes 文件 `USER.md`、`MEMORY.md` 和 `SOUL.md` 的更完整工作流
- [x] 原生 session 工作流，包含更清晰的元数据、搜索、删除和进入页面刷新
- [x] usage dashboard，包含 token 总量、热门 sessions、热门 models、趋势和可用时的主机级多 profile 总计
- [x] 原生 skill 工作流，用于发现、查看、创建和编辑 `~/.hermes/skills/` 下的远程 `SKILL.md`
- [x] 与同一 SSH 目标上的 Hermes Agent profiles 对齐的 profile 感知主机工作流
- [x] 远程规范 scheduler state 的原生 cron job 工作流
- [x] 真实内嵌 SSH 终端，包含标签页、外观控制和一致的多 profile 工作区行为
- [x] Apple Silicon 和 Intel 的 universal macOS 发布打包，以及打包流程中的 bundle version stamping

### 后续方向

- 通过 signing 和 notarization 降低分发摩擦
- 持续打磨 onboarding、诊断、终端 UX 和多主机细节，同时不加入第二种传输模型或影子状态

更大的功能应由 Hermes 本身的需要来证明，而不是为了新奇而加入。

## 从源码构建

本地开发推荐直接构建 app bundle：

```bash
./scripts/build-macos-app.sh
```

然后打开 `dist/HermesDesktop.app`。

运行 release 支持测试：

```bash
./scripts/run-tests.sh
```

创建 GitHub Releases 归档：

```bash
./scripts/package-github-release.sh
```

为 release candidate 打包时，可以显式写入版本：

```bash
HERMES_VERSION=0.5.0 ./scripts/package-github-release.sh
```

发布产物：

- `dist/HermesDesktop.app.zip`，适用于 Apple Silicon 和 Intel Mac 的 universal macOS archive
