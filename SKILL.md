---
name: repair-computer-use-windows
description: Diagnose and repair Windows Codex Desktop Computer Use when it is unavailable, becomes unavailable after working for a while, reports a missing native pipe or helper path, loses bundled plugin files, or logs EBUSY while refreshing openai-bundled. Use for Computer Use plugin cache, marketplace, stable latest junction, Chrome native messaging host, config BOM, sandbox error 740, and runtime injection verification problems.
---

# Repair Windows Computer Use

Repair only Computer Use and its related bundled browser state. Do not repatch unrelated Codex features unless explicitly requested.

## Security

- Never read or print `auth.json`.
- Never print account data, cookies, tokens, API keys, authorization headers, or the full environment.
- Read only named non-secret environment variables required by this workflow.
- Filter logs to the fixed Computer Use error markers listed below. Do not dump whole logs.
- Report backup paths without displaying backup contents.
- Redact unexpected credential-like values before reporting output.

## Safety

- Run only on Windows.
- Back up Codex state before modifying configuration or plugin files.
- Do not modify `C:\Program Files\WindowsApps` in place.
- Do not change `[windows].sandbox` without evidence of OS error 740.
- Do not treat a missing pipe in an already-running `node_repl` process as proof that repair failed.
- Do not run `codex plugin add browser@openai-bundled` or `chrome@openai-bundled` after stable junction repair. The CLI backup operation can damage or reject the `latest` junction.

## Workflow

### 1. Back Up

Run:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<skill-root>\scripts\manage-codex-backups.ps1" -Action Backup
```

Record only the returned backup path.

### 2. Diagnose

Run strict verification:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<skill-root>\scripts\repair-computer-use.ps1" -StrictVerifyOnly
```

Search current-day Desktop logs for only:

```text
bundled_plugins_marketplace_resolve_failed
EBUSY
computer-use native pipe helper paths changed
missing-helper-path
computer-use native pipe startup failed
computer-use native pipe startup ready
```

Logs are under:

```text
%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs\<year>\<month>\<day>
```

The characteristic recurring failure is:

```text
startup ready
-> EBUSY on .tmp\bundled-marketplaces\openai-bundled\plugins\chrome
-> helper paths changed
-> missing-helper-path
```

This means the Chrome extension host locked the mutable marketplace mirror.

### 3. Repair

Run:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<skill-root>\scripts\repair-computer-use.ps1" -VerifyOnly
```

The script repairs automatically when verification fails. It:

- Stops bundled `extension-host` lock holders.
- Rebuilds the `openai-bundled` mirror from the current Codex package.
- Copies `browser` and `chrome` into stable versioned cache directories.
- Repoints their `latest` junctions to stable directories.
- Rewrites the Chrome Native Messaging manifest to a stable versioned host path.
- Installs the local Computer Use compatibility plugin.
- Enables the current-user Computer Use environment gate.
- Backs up `config.toml` before writes.
- Tests helper transport with a screenshot request.

### 4. Validate Configuration

Confirm without printing unrelated configuration:

- `config.toml` parses as TOML.
- Its first three bytes are not `EF-BB-BF`.
- `[features] computer_use = true`.
- `[plugins."computer-use@openai-bundled"] enabled = true`.
- `CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE=1` exists for the current user.
- `browser/latest` and `chrome/latest` target versioned caches, not `.tmp`.
- Native Messaging points to `chrome\<version>\extension-host\...`, not `chrome\latest` or `.tmp`.

Only set:

```toml
[windows]
sandbox = "unelevated"
```

when sandbox logs explicitly show `spawn setup refresh` with OS error 740.

### 5. Verify

Run strict verification again:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<skill-root>\scripts\repair-computer-use.ps1" -StrictVerifyOnly
```

Confirm Desktop logs end with:

```text
computer-use native pipe startup ready
```

For a fresh runtime, inspect only:

- `nodeRepl.env.SKY_CUA_NATIVE_PIPE_DIRECTORY`
- `nodeRepl.nativePipe`
- `nodeRepl.nativePipe.createConnection`

Then import the absolute Computer Use client path and call `sky.list_apps()`.

If the current `node_repl` host started before repair, `js_reset` does not refresh native-pipe injection. Trust `startup ready` plus strict helper verification, then verify `sky.list_apps()` in a new session or after Codex restarts.

## Report

Report in Chinese:

- The evidenced root cause.
- Backup paths.
- Files, junctions, manifests, and configuration changed.
- Strict verification and helper transport results.
- Whether Desktop logs show `startup ready`.
- Any remaining old-session limitation.

Never include secrets or unrelated log/config content.
