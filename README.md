# AKDAgent Plugin

在 **Codex、Claude Code、Claude Desktop、Cursor、VS Code Copilot 或 OpenCode** 中，用自然语言检查、填词和调教 Synthesizer V 工程。通过本地 MCP 编辑服务和 Lua 桥，将操作反馈到 SV 界面。

维护者：[littleclock2](https://github.com/littleclock2) · 基于 [Akunda123 的 AKDAgent](https://github.com/Akunda123/SVIXAGENT) · [English](README.en.md)

这是插件的独立维护与下载仓库，包含适配器、音乐工作流、客户端配置及部署脚本。AKDAgent 编辑服务、Lua 桥及选定作者资料来自固定源码提交，随发行附件独立提供；不需要安装 AKDAgent 客户端。

## 快速安装（Windows，个人范围）

**推荐：独立一键部署**。从 [Releases](https://github.com/littleclock2/AKDAgent-Plugin/releases) 下载 `akdagent-one-click-0.3.0.zip`，完整解压到可写目录，双击根目录 `Deploy.cmd`，选择要接入的应用。

**也可以将[本仓库链接](https://github.com/littleclock2/AKDAgent-Plugin)直接发给你的 AI Agent，让它按照安装说明帮你完成安装。**

前提是 Windows x64、支持脚本的 Synthesizer V、所需声库及支持本地 MCP 的客户端。首次部署需要联网安装依赖；缺少可用 Node 时，安装器下载并校验固定版本，不修改系统 PATH。

部署后重启所选应用、启用插件或 MCP 连接，并在目标 SV 中运行 `脚本 → Agent → AKDAgent Bridge`。一次只使用一个活跃编辑客户端和 SV 桥。先发送“检查当前 SV 工程，不要修改”，确认读取正常后，在工程副本上编辑、回读、试听并自行保存。

也可分别下载插件、MCP 服务和资料三个 ZIP，解压为同级目录。完整步骤见 [一键部署](plugins/akdagent-chatgpt-plugin/docs/standalone.md) 和 [各客户端接入](plugins/akdagent-chatgpt-plugin/docs/clients.md)。GitHub 自动生成的 Source code ZIP 是源码，不是可直接运行的一键安装包。

## 功能

- 工程上下文、轨道、音符与选区读取，按桥心跳识别 SV1 / SV2。
- 使用 AKDAgent 工具处理歌词、音素、音符音高与时值、音高曲线、气声、张力和响度等参数。
- 实时操作、调教、歌词、作曲、工程文件五套工作流，按任务读取作者资料；无技能加载机制的客户端也可通过 MCP 读取。
- 本次服务含 53 个 AKDAgent 工具及 4 个集成工具，共 57 个；具体可用功能受服务版本、宿主、声库及附加组件影响。

完整功能及条件见 [插件 README](plugins/akdagent-chatgpt-plugin/README.md)。对话使用客户端当前选择的模型，无需另配 AKDAgent 模型聊天的 API key；所选客户端和模型的额度与计费规则仍然适用。

## 验证与使用边界

标准清单校验、Claude Code 严格插件校验、七种配置的 MCP 初始化与读取，以及 OpenCode 1.18.34 实际连接已验证；本地八项自动化测试通过。部分客户端界面、模型驱动编辑、SV1 / macOS 和本地模型推理仍待验收，不把连接成功等同于音乐效果或工程已保存。

MDX-Net / CREPE 推理、Dolce 识谱，以及 Instrument X、Flat、ACE Studio 等功能需要相应模型、引擎或宿主。没有 SV 侧栏聊天同步功能。详见 [测试记录](plugins/akdagent-chatgpt-plugin/docs/testing.md) 和 [安全与隐私](plugins/akdagent-chatgpt-plugin/SECURITY.md)。

0.3.0 以预览版分发。2026-10-07 对锁定生产依赖执行 npm audit，报告 14 项（3 moderate、7 high、4 critical，含间接/传播项），尚未完成可达性判断和安全升级回归；本次保留 AKDAgent 的锁文件，没有自动修复。仅用于可信本地环境，具体风险见安全说明。

## 从源码构建发行包

仅使用插件的用户请选择 Release 一键包；下面流程面向开发者。需要 Git 和带 npm 的 Node 22.13+。在本仓库根目录执行：

```powershell
$ErrorActionPreference = 'Stop'
$sourceInfo = Get-Content -LiteralPath .\UPSTREAM.json -Raw | ConvertFrom-Json
git clone --no-checkout $sourceInfo.repository .\vendor\SVIXAGENT
if ($LASTEXITCODE -ne 0) { throw 'Source clone failed.' }
git -C .\vendor\SVIXAGENT checkout --detach $sourceInfo.commit
if ($LASTEXITCODE -ne 0) { throw 'Pinned source checkout failed.' }
npm --prefix .\vendor\SVIXAGENT\server ci --ignore-scripts
if ($LASTEXITCODE -ne 0) { throw 'Dependency installation failed.' }
npm --prefix .\vendor\SVIXAGENT\server run build
if ($LASTEXITCODE -ne 0) { throw 'Service build failed.' }
powershell -NoProfile -ExecutionPolicy Bypass -File .\plugins\akdagent-chatgpt-plugin\scripts\package.ps1 -SourceRoot .\vendor\SVIXAGENT -OutputDirectory .\dist\release-0.3.0
if ($LASTEXITCODE -ne 0) { throw 'Packaging failed.' }
```

已有 `vendor/SVIXAGENT` 时先核对源码版本，不要覆盖；输出目录已存在时打包停止。源码与构建产物只在本地 `vendor/`、`dist/` 中生成，不提交到插件仓库。不需要启动 SV，也不下载推理模型。打包包含 Lua 桥、四个 ZIP 和 `SHA256SUMS.txt`，资料来源提交取自指定 AKDAgent checkout。

贡献和测试命令见 [CONTRIBUTING](plugins/akdagent-chatgpt-plugin/CONTRIBUTING.md)。插件标识 `akdagent-chatgpt-plugin` 为兼容保留，展示名称为 AKDAgent Plugin；插件版本不等同于 AKDAgent 客户端、服务或 Lua 桥版本。

## 来源与许可

适配器及技能入口采用 [MIT](LICENSE)，贡献者署名为 littleclock2，保留 Akunda123 的原有署名。参考资料保留各自来源与混合许可，包括 SynthVCopilot 的附加条款，不能把资料包整体宣称为纯 MIT。详见 [第三方说明](plugins/akdagent-chatgpt-plugin/THIRD-PARTY-NOTICES.md) 和 [资料包说明](plugins/akdagent-chatgpt-plugin/docs/reference-pack.md)。

集成代码和文档使用了生成式 AI 辅助。社区集成是否被 [AKDAgent](https://github.com/Akunda123/SVIXAGENT) 收录由维护者决定；本仓库不代表平台或原作者背书。
