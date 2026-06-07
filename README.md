# Repair Windows Computer Use

[English](#english) | [中文](#中文)

## English

A Codex skill for diagnosing and repairing recurring Windows Codex Desktop Computer Use failures.

### Features

- Repairs `openai-bundled` marketplace corruption.
- Resolves `EBUSY` caused by Chrome Native Messaging locking the mutable marketplace mirror.
- Repairs stable `browser/latest` and `chrome/latest` cache junctions.
- Repairs Computer Use helper path and native-pipe startup failures.
- Repairs the Chrome Native Messaging manifest.
- Checks for a UTF-8 BOM and Windows sandbox error 740.
- Performs strict helper transport verification.

### Install

Place this repository in your Codex skills directory:

```text
%USERPROFILE%\.codex\skills\repair-computer-use-windows
```

Then invoke:

```text
$repair-computer-use-windows
```

### Security

The skill explicitly forbids reading or printing `auth.json`, account data, cookies, tokens, API keys, authorization headers, or the full environment.

### Upstream Attribution

The repair scripts were adapted from the Computer Use repair workflow in:

<https://github.com/chen0416ccc-cpu/codex-windows-fast-patch-skill>

That upstream repository did not expose a license when this repository was prepared. No additional license is asserted over adapted upstream code.

## 中文

这是一个用于诊断和修复 Windows 版 Codex Desktop Computer Use 反复不可用问题的 Codex Skill。

### 功能

- 修复 `openai-bundled` marketplace 损坏。
- 解决 Chrome Native Messaging 锁定可变 marketplace 镜像所导致的 `EBUSY`。
- 修复稳定的 `browser/latest` 和 `chrome/latest` 缓存 junction。
- 修复 Computer Use helper 路径和 native pipe 启动失败。
- 修复 Chrome Native Messaging manifest。
- 检查 UTF-8 BOM 和 Windows sandbox error 740。
- 执行严格的 helper transport 验证。

### 安装

将此仓库放入 Codex Skills 目录：

```text
%USERPROFILE%\.codex\skills\repair-computer-use-windows
```

然后调用：

```text
$repair-computer-use-windows
```

### 安全

此 Skill 明确禁止读取或输出 `auth.json`、账号信息、Cookie、token、API key、Authorization header 或完整环境变量。

### 上游归属

修复脚本改编自以下仓库中的 Computer Use 修复流程：

<https://github.com/chen0416ccc-cpu/codex-windows-fast-patch-skill>

在准备本仓库时，上游仓库未公开许可证。对于改编自上游的代码，本仓库不额外主张许可证授权。
