# wechat-cli

`wechat-cli` 是一个用于查询微信数据的命令行工具，可以读取：

- 最近会话
- 联系人和群聊
- 指定聊天的历史消息
- 消息搜索结果
- 增量新消息

当前仓库可在 **macOS arm64** 环境下直接使用，并且已经验证可以正常获取微信会话、联系人和聊天消息。

## 适用环境

使用前请先确认：

- 操作系统：**macOS**
- CPU 架构：**Apple Silicon / arm64**
- `Node.js` 版本不低于 `14`
- 本机已经安装并登录微信客户端

## 获取项目

先把仓库克隆到本地：

```bash
git clone https://github.com/billjohnson990522/wechat-cli.git
cd wechat-cli
```

## 使用方式

这个仓库目前推荐两种使用方式。

### 方式一：直接在仓库目录中运行

如果你已经克隆了仓库，并且希望直接使用仓库内已有文件，这是最简单的方式：

```bash
node ./bin/wechat-cli.js --help
```

例如查看最近会话：

```bash
node ./bin/wechat-cli.js sessions --limit 5 --format json
```

### 方式二：先打包，再作为 npm 包安装

如果你希望把它当成一个 npm 包安装到其它目录，可以先在仓库根目录执行：

```bash
npm pack
```

然后在目标目录安装：

```bash
npm install ./canghe_ai-wechat-cli-0.2.4.tgz
```

安装完成后，使用方式如下：

```bash
./node_modules/.bin/wechat-cli --help
./node_modules/.bin/wechat-cli sessions --limit 5 --format json
```

## 首次初始化

首次使用通常需要先初始化：

```bash
node ./bin/wechat-cli.js init
```

如果你已经把它安装成 npm 包，也可以写成：

```bash
wechat-cli init
```

初始化完成后，会在用户目录下生成配置目录：

```bash
~/.wechat-cli/
```

常见文件包括：

- `config.json`
- `all_keys.json`
- `monitor-groups.json`

## 常用命令

### 1. 查看最近会话

```bash
node ./bin/wechat-cli.js sessions
node ./bin/wechat-cli.js sessions --limit 10
node ./bin/wechat-cli.js sessions --limit 10 --format json
```

### 2. 搜索联系人或群聊

```bash
node ./bin/wechat-cli.js contacts --query "慧博"
node ./bin/wechat-cli.js contacts --query "李" --format json
```

### 3. 查看聊天记录

```bash
node ./bin/wechat-cli.js history "张三"
node ./bin/wechat-cli.js history "AI交流群" --limit 20
node ./bin/wechat-cli.js history "AI交流群" --start-time "2026-04-01" --end-time "2026-04-02"
node ./bin/wechat-cli.js history "AI交流群" --limit 20 --format json
```

### 4. 搜索消息内容

```bash
node ./bin/wechat-cli.js search "Claude"
node ./bin/wechat-cli.js search "你好" --limit 50
node ./bin/wechat-cli.js search "回测" --chat "AI交流群"
```

### 5. 获取增量新消息

```bash
node ./bin/wechat-cli.js new-messages
```

### 6. 查询群成员

```bash
node ./bin/wechat-cli.js members "AI交流群"
```

### 7. 导出聊天记录

```bash
node ./bin/wechat-cli.js export "AI交流群"
```

## 快速验证

如果你想快速确认当前环境是否可用，可以依次执行：

```bash
node ./bin/wechat-cli.js --help
node ./bin/wechat-cli.js sessions --limit 5 --format json
node ./bin/wechat-cli.js contacts --query "慧博" --format json
node ./bin/wechat-cli.js history "国新&慧博AI业务沟通群" --limit 3 --format json
```

只要这些命令能正常返回结果，就说明当前环境已经具备基本可用性。

## 注意事项

### 1. 当前仓库主要面向 macOS arm64

仓库中已经包含的可执行文件位于：

```bash
node_modules/@canghe_ai/wechat-cli-darwin-arm64/bin/wechat-cli
```

因此当前最稳妥的运行平台是 **macOS arm64**。

### 2. `npm pack` 生成的压缩包默认不包含仓库内的 `node_modules`

当前 `package.json` 的 `files` 配置只包含：

- `bin/`
- `install.js`

所以 `npm pack` 之后生成的 tarball，主要包含包装脚本和安装脚本。安装时如果网络可用，npm 会尝试根据 `optionalDependencies` 自动拉取：

```bash
@canghe_ai/wechat-cli-darwin-arm64@0.2.4
```

如果你希望在完全离线的环境里安装使用，就不能只依赖 `npm pack` 产物，还需要额外保留平台二进制。

### 3. 工具依赖本机微信状态

这个工具不是纯离线文件解析器，它依赖：

- 本机微信客户端
- 当前用户的微信数据环境
- 初始化得到的本地配置和密钥信息

如果微信没有登录，或者初始化没有完成，命令可能无法返回消息数据。

## 当前仓库包含的关键文件

当前仓库中最重要的文件包括：

- `package.json`
- `bin/wechat-cli.js`
- `install.js`
- `node_modules/@canghe_ai/wechat-cli-darwin-arm64/bin/wechat-cli`
- `node_modules/@canghe_ai/wechat-cli-darwin-arm64/package.json`

其中：

- `bin/wechat-cli.js` 是 Node 包装脚本
- `install.js` 用于处理平台二进制权限
- `node_modules/@canghe_ai/wechat-cli-darwin-arm64/bin/wechat-cli` 是 macOS arm64 平台的可执行文件

## 说明

这个仓库是一个可运行恢复版仓库，目标是保证别人拿到仓库后可以理解如何安装和使用这个包。

如果你后续还要把它整理成更标准的发布结构，例如：

- 去掉仓库内置 `node_modules`
- 调整 `package.json` 的 `files`
- 做成真正适合离线分发的包结构

可以在这个基础上继续整理。
