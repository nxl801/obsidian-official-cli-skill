# Obsidian Official CLI Skill for OpenClaw（中文说明）

[English version / 英文版](./README.md)

这是一个面向 OpenClaw 的 AgentSkill，用来调用 **Obsidian 1.12+ 官方 CLI**（`obsidian`）。

它的核心定位是：让 OpenClaw、Claude Code 以及其他 AI Agent 以 **retrieval-first（先检索、后阅读）** 的方式访问一个正在运行的 Obsidian 桌面应用，而不是一上来就走整库向量化导入。

## 为什么要做这个 skill

在 Obsidian 生态里，过去已经有一些社区工具，例如：
- `obsidian-cli`
- NotesMD / 旧版 obsidian-cli 路线

它们在某些场景仍然有价值。但从 Obsidian 1.12 开始，官方已经提供了自己的 CLI 接口。对 AI Agent 来说，这意味着可以直接利用 Obsidian 自己的搜索和结构能力。

这个 skill 的推荐链路是：

```text
search -> search:context -> read -> backlinks / links / properties
```

这条链路的优势是：
- 检索过程更透明
- 结果更容易验证
- 更贴近人类在 Obsidian 里看到的内容
- 通常比“先全量导入再检索”更省 token

## Skill 覆盖的核心命令

本 skill 主要围绕这些官方 CLI 命令展开：

- `search`
- `search:context`
- `read`
- `outline`
- `backlinks`
- `links`
- `tags`
- `properties`
- `tasks`
- `unresolved`

默认采用 **只读优先** 的策略，同时对 `append`、`delete`、`command`、`eval` 这类写入或高风险命令明确了边界。

## 仓库内容

- `obsidian-official-cli/` — skill 源码
- `dist/obsidian-official-cli.skill` — 打包后的 skill 文件
- `README.md` — 英文 README

## 使用前提

- Obsidian Desktop **1.12+**
- 已在 Obsidian 中启用：
  - `Settings -> General -> Command line interface`
- 系统中可以直接调用 `obsidian` 命令

在 macOS 上，必要时可以使用登录 shell，确保 PATH 包含：

```text
/Applications/Obsidian.app/Contents/MacOS
```

## 示例用法

### 按主题找笔记

```bash
obsidian search query="PLC" limit=10 format=json
obsidian search:context query="PLC" limit=10 format=json
```

### 读取具体笔记

```bash
obsidian read path="文档发布/2026-03-08_边缘AI推理安全与网关防护.md"
```

### 扩展到笔记关系图

```bash
obsidian backlinks file="Central note" format=json counts
obsidian links file="Central note"
```

### 读取 vault 结构信息

```bash
obsidian tags counts format=json
obsidian properties counts format=json
obsidian unresolved total
```

## 安装方式

### 方式一：使用打包产物

直接使用：

```text
dist/obsidian-official-cli.skill
```

### 方式二：复制源码目录

把下面这个目录复制到 OpenClaw skills 目录：

```text
obsidian-official-cli/
```

## 设计取舍

这个 skill **并不是为了替换** 老的社区 `obsidian-cli` / NotesMD 工作流，而是为那些已经启用了 **官方 `obsidian` CLI** 的环境提供一条更自然的 Agent 路线。

它刻意偏向：
- 小步、可解释的检索流程
- 尽量使用结构化输出（例如 `format=json`）
- 先缩小范围，再读正文

## 当前状态

- 已通过校验
- 已完成打包
- 已在真实 Obsidian vault 上完成官方 CLI 测试

## License

MIT
