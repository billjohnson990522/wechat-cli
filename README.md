# wechat-cli

这是一个恢复版 `wechat-cli` 仓库，用来保留本机仍然可用的 `@canghe_ai/wechat-cli@0.2.4` 安装内容。

当前仓库已经验证可以正常读取微信会话、联系人和聊天消息。

## 项目说明

- 包名：`@canghe_ai/wechat-cli`
- 当前版本：`0.2.4`
- 当前仓库来源：从本机已安装目录恢复得到
- 适用平台：**macOS arm64**

这个仓库不是从原始源码仓库完整克隆下来的版本，而是从本机仍然存在的 npm 安装内容恢复出来的版本。因此它更适合作为：

- 可运行备份
- 可归档备份
- 可继续验证使用的恢复版项目

如果原始上游仓库里曾经存在未打进 npm 包的源码文件，这个仓库里不一定全部包含。

## 仓库内容

当前仓库中最关键的文件有：

- `package.json`
- `bin/wechat-cli.js`
- `install.js`
- `node_modules/@canghe_ai/wechat-cli-darwin-arm64/bin/wechat-cli`
- `node_modules/@canghe_ai/wechat-cli-darwin-arm64/package.json`

其中：

- `bin/wechat-cli.js` 是 Node 包装脚本
- `install.js` 会在安装时处理平台二进制权限
- `node_modules/@canghe_ai/wechat-cli-darwin-arm64/bin/wechat-cli` 是当前仓库内已恢复出来的 macOS arm64 可执行文件

## 环境要求

使用前请先确认：

- 操作系统是 **macOS**
- CPU 架构是 **Apple Silicon / arm64**
- 已安装 `Node.js`，版本不低于 `14`
- 本机已经登录微信客户端

## 安装方式

### 方式一：直接在当前仓库里使用

这是最直接的方式，适合本机已经包含恢复出来的二进制文件时使用。

```bash
cd /path/to/wechat-cli
node ./bin/wechat-cli.js --help
```

也可以直接执行常用命令：

```bash
cd /path/to/wechat-cli
node ./bin/wechat-cli.js sessions --limit 5 --format json
```

### 方式二：作为 npm 包安装

如果你要把它当作 npm 包来安装，可以先在仓库根目录打包：

```bash
npm pack
```

然后在目标目录安装：

```bash
npm install ./canghe_ai-wechat-cli-0.2.4.tgz
```

安装完成后可以这样调用：

```bash
./node_modules/.bin/wechat-cli --help
./node_modules/.bin/wechat-cli sessions --limit 5 --format json
```

## 首次初始化

首次使用通常需要先执行初始化：

```bash
wechat-cli init
```

如果你是在当前仓库里直接运行，可以写成：

```bash
node ./bin/wechat-cli.js init
```

初始化完成后，会在用户目录下生成配置文件，通常位于：

```bash
~/.wechat-cli/
```

常见文件包括：

- `config.json`
- `all_keys.json`
- `monitor-groups.json`

## 常用命令

### 查看最近会话

```bash
wechat-cli sessions
wechat-cli sessions --limit 10
wechat-cli sessions --limit 10 --format json
```

### 搜索联系人

```bash
wechat-cli contacts --query "慧博"
wechat-cli contacts --query "李" --format json
```

### 查看聊天记录

```bash
wechat-cli history "张三"
wechat-cli history "AI交流群" --limit 20
wechat-cli history "AI交流群" --start-time "2026-04-01" --end-time "2026-04-02"
wechat-cli history "AI交流群" --limit 20 --format json
```

### 搜索消息内容

```bash
wechat-cli search "Claude"
wechat-cli search "你好" --limit 50
wechat-cli search "回测" --chat "AI交流群"
```

### 获取增量消息

```bash
wechat-cli new-messages
```

### 查看群成员

```bash
wechat-cli members "AI交流群"
```

### 导出聊天记录

```bash
wechat-cli export "AI交流群"
```

## 在当前仓库中的运行示例

下面这些命令已经在当前恢复版仓库中实际验证通过：

```bash
node ./bin/wechat-cli.js sessions --limit 3 --format json
node ./bin/wechat-cli.js contacts --query "慧博" --format json
node ./bin/wechat-cli.js history "国新&慧博AI业务沟通群" --limit 3 --format json
```

验证结果表明：

- 可以读取最近微信会话
- 可以搜索联系人和群聊
- 可以读取指定群聊的历史消息

## 注意事项

### 1. 这是恢复版，不是完整源码版

这个仓库的核心目标是“保住可运行版本”，不是保证还原出完整上游开发源码。

### 2. 当前恢复内容以 macOS arm64 为主

仓库中已经包含的可执行文件是：

```bash
node_modules/@canghe_ai/wechat-cli-darwin-arm64/bin/wechat-cli
```

因此当前最稳妥的运行平台是 **macOS arm64**。

### 3. `npm pack` 打出来的 tarball 默认不内置平台二进制

当前 `package.json` 的 `files` 只包含：

- `bin/`
- `install.js`

所以 `npm pack` 之后生成的压缩包，默认主要包含包装脚本，不会把当前仓库里的 `node_modules` 一起打进去。安装时如果网络可用，npm 会尝试根据 `optionalDependencies` 拉取：

```bash
@canghe_ai/wechat-cli-darwin-arm64@0.2.4
```

如果你希望“离线安装也能直接运行”，需要额外调整打包方式或发布结构。

### 4. 依赖本机微信状态

这个工具不是纯离线解析工具，它依赖：

- 本机微信客户端
- 当前用户的微信数据环境
- 初始化得到的本地配置和密钥信息

如果微信没有登录，或者本地环境没有初始化好，命令可能无法返回消息数据。

## 快速自检

如果你想快速确认当前环境是否可用，可以依次执行：

```bash
node ./bin/wechat-cli.js --help
node ./bin/wechat-cli.js sessions --limit 5 --format json
node ./bin/wechat-cli.js contacts --query "慧博" --format json
node ./bin/wechat-cli.js history "国新&慧博AI业务沟通群" --limit 3 --format json
```

只要这几步能正常返回，就说明当前恢复版项目基本可用。
