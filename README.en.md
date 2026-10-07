# AKDAgent Plugin

Inspect and edit Synthesizer V projects from Codex, Claude Code, Claude Desktop, Cursor, VS Code Copilot or OpenCode through a local MCP service and Lua bridge.

Maintainer: [littleclock2](https://github.com/littleclock2). Based on [AKDAgent by Akunda123](https://github.com/Akunda123/SVIXAGENT). [中文](README.md) · [Detailed English guide](plugins/akdagent-chatgpt-plugin/README.en.md).

## Quick installation

Recommended: download `akdagent-one-click-0.3.0.zip` from [Releases](https://github.com/littleclock2/AKDAgent-Plugin/releases), extract it completely into a writable directory, run `Deploy.cmd` and select the target applications. No AKDAgent client installation is required.

**You can also send [this repository's link](https://github.com/littleclock2/AKDAgent-Plugin) to your AI agent and ask it to follow the installation guide for you.**

Prerequisites: Windows x64, a script-capable Synthesizer V host, a voicebank and a local MCP client. First-time deployment needs network access for locked dependencies and, if necessary, checksum-pinned Node. Enable the client connection and start **Agent → AKDAgent Bridge** in SV. Use one active editing client/bridge and begin with a read-only inspection. Work on a project copy, read back and audition changes, then save in the host.

The GitHub Source code archives are source distributions, not one-click deployment bundles. See [deployment](plugins/akdagent-chatgpt-plugin/docs/standalone.md) and [client setup](plugins/akdagent-chatgpt-plugin/docs/clients.md).

## Capabilities and verification

The integration provides project/selection inspection, note/lyric/phoneme editing, pitch curves and expression parameters through AKDAgent's unchanged tool implementations. Five workflows cover live editing, vocal tuning, lyrics, composition and project files; MCP readers make them available to clients without skill loading. This service lists 53 existing tools plus four integration tools, totaling 57.

Manifest validation, Claude Code strict validation, seven MCP-profile launch/read checks, OpenCode 1.18.34 connection and eight local automated tests passed. Some client UI/model-driven editing, SV1/macOS and inference-model features remain acceptance checks. Connection does not prove audio quality or saved projects. Optional inference/OMR and other-host tools need their models, engines or hosts. There is no SV sidebar chat synchronization.

No AKDAgent model-chat API key is required. Your selected client/model's limits and billing still apply. See [verification](plugins/akdagent-chatgpt-plugin/docs/testing.md) and [security/privacy](plugins/akdagent-chatgpt-plugin/SECURITY.md).

Version 0.3.0 is a preview release. A 2026-10-07 production-dependency npm audit reported 14 affected entries (3 moderate, 7 high, 4 critical, including transitive/propagated findings). Reachability and security-upgrade regression checks remain outstanding; this release retains AKDAgent's lockfile without automatic fixes. Use only in trusted local environments and read the security notes.

## Source builds and attribution

This repository maintains the adapter, workflows and deployment tools, not a copy of the complete AKDAgent client. `UPSTREAM.json` pins the service/bridge/reference source. The [Chinese README](README.md#从源码构建发行包) contains reproducible PowerShell build commands using a separate checkout and `package.ps1 -SourceRoot`; Git and Node 22.13+ with npm are required. Local `vendor/` and `dist/` are ignored. See [contributing](plugins/akdagent-chatgpt-plugin/CONTRIBUTING.md).

Integration files use [MIT](LICENSE), attributed to littleclock2 while retaining Akunda123's original attribution. References retain mixed third-party terms, including SynthVCopilot's additional terms; the entire reference pack is not represented as MIT. See [notices](plugins/akdagent-chatgpt-plugin/THIRD-PARTY-NOTICES.md). Integration code/documentation used generative-AI assistance. This community integration does not imply platform or original-author endorsement.
