---
name: reverse-engineer-anything
description: Reverse engineer native binaries, managed assemblies, Electron/JavaScript apps, packaged archives, firmware images, network/signal captures, and web applications using the REA workflow. Use when the user asks to reverse engineer, decompile, unpack, inspect binaries/APKs/firmware/PCAPs/HARs/WASM, generate rea-agents CLI commands, interpret REA Evidence JSON, or build AgentEnv web bundles.
---

# Reverse Engineer Anything

Reverse engineer compiled binaries, packaged apps, firmware, network/signal captures, and runtime web targets using strict evidence-backed methodology and the `rea-agents` toolkit.

## When to Use

- The user wants to reverse engineer, decompile, unpack, or inspect a shipped binary, package, firmware image, APK, ASAR archive, or WebAssembly module.
- The user wants to analyze recorded traffic or hardware traces (PCAP, HAR, SAZ, mitmproxy, aioquic qlog, or sigrok logic captures).
- The user wants to capture a live web application workflow into an AgentEnv bundle or inspect an Electron runtime.
- The user pastes JSON `Evidence` output, decompiled pseudocode, or disassembly from `rea-agents`, Ghidra, JADX, or TShark and needs structured analysis or reconstruction guidance.
- Skip this skill for ordinary architecture analysis of complete, uncompiled source repositories.

## Step 1: Detect Execution Environment

Determine which capabilities are available in the current session before selecting an action:

1. **Connected REA MCP Environment (e.g., Gemini CLI with `rea-agents` MCP registered):**
   - Proceed directly to calling the matching REA MCP tool from the routing matrix below.
   - Do not run setup or diagnostics unless a tool call fails.
   - If an MCP call times out or drops, retry once only if the operation is read-only and idempotent. Never blindly retry a state-mutating runtime action.

2. **Sandbox / Chat Environment (No REA MCP Tools Available):**
   - **If the user uploaded a file** that can be inspected statically with standard sandbox tools (e.g., JS bundles, JSON/HAR captures, ZIP/APK file inventories, text configs, or disassembly dumps), analyze the artifact directly and record exact SHA-256 digests and byte offsets.
   - **If the target requires a local native engine** (Ghidra, IDA, JADX, Binwalk, Unblob, TShark, sigrok-cli, WABT, or local browser/Electron attachment), do not pretend to execute the tool. Instead, output the exact copy-paste `npx -y rea-agents@latest` or native CLI command for the user to run on their machine, specify which output or JSON fields to bring back, and analyze the returned evidence in the next turn.

## Step 2: Route the Target

Select the workflow that matches the user's artifact or runtime target:

### Packaged Apps, Scripting & WebAssembly

- **ASAR or extracted JavaScript/Electron tree:**
  - MCP tool: `analyze_javascript_application`
  - CLI command: `npx -y rea-agents@latest analyze-javascript-application /absolute/path/to/app --json`
- **Archive or package member inventory (ZIP, APK, IPA, MSIX, AppX, DMG, PyInstaller executable):**
  - MCP tool: `open_binary` with `detail: "summary"` first for large archives, followed by `inspect_artifact` or retained inventory views.
  - Note: For PyInstaller executables, the CArchive is the application layer; the native prefix is only the bootloader.
- **Apple bundle resources (plist, Interface Builder nibs/storyboards, asset catalogs, keyed archives):**
  - MCP tool: `open_bundle_resources` followed by the specific resource inspection tool. Use `open_binary` only when inspecting native Mach-O code.
- **WebAssembly (`.wasm`) artifact:**
  - MCP tool: `inspect_wasm_artifact` (requires WABT 1.0.42 via `REA_WABT_BIN_DIRECTORY`).
  - Explicitly link JavaScript glue candidates and preserve URL/path ambiguity. Treat decoded WAT as a disassembly representation, never as original source code.

### Mobile, Managed & Native Binaries

- **Android APK / AAB code, classes, and cross-references:**
  - MCP tool: `inspect_android_package` and focused Android follow-up tools.
- **Android APK / ZIP / AAB cryptographic signing:**
  - MCP tool: `inspect_apk_signature` (use `inspect_analysis_view` with `kind: "apk-signature"` for follow-ups).
  - Verify scheme presence (v1 JAR, v2, v3, v3.1), certificate SHA-256 fingerprints, v3 key-rotation lineage, and digest self-consistency.
- **Managed .NET PE / CLI assembly:**
  - MCP tool: `inspect_managed_artifact`.
- **Native PE / ELF / Mach-O or .NET NativeAOT binary (Static):**
  - MCP tool: `open_binary` and `trace_native_values` backed by Ghidra or IDA.
  - Establish static control-flow, string references, imports/exports, and decompiled pseudocode before proposing dynamic instrumentation.
- **Native program live behavior or instrumentation (Dynamic):**
  - Use dedicated native runtime observation tools. Never claim runtime execution behavior solely from `open_binary` or `trace_native_values`.
- **Firmware image:**
  - MCP tool: `inspect_firmware_regions` to map layout, and `extract_firmware` with a new absolute output directory when extraction is requested (uses Binwalk/Unblob on Linux).

### Browser, Electron & Network / Signal Captures

- **Already-open user browser tab:**
  - MCP tool: `list_browser_targets`.
- **Automated browser interaction or AgentEnv bundle creation:**
  - MCP tool: `capture_browser_scenario` with declared actions (click, type, navigate), followed by `export_environment_bundle` when building an AgentEnv bundle.
- **Running Electron application:**
  - MCP tool: `list_electron_targets`. Always run `inspect_electron_fuses` before attempting an `electron-main` Inspector attach; if `EnableNodeCliInspectArguments` is disabled, `--inspect` is ignored by the binary.
- **Saved web network capture (HAR, Fiddler `.saz`, mitmproxy, aioquic `.qlog`):**
  - MCP tool: `inspect_web_network_capture` (start with `detail: "summary"`).
  - Do not fetch recorded URLs live or claim live server attribution from historical captures. If decoding large captures fails due to heap limits, advise setting `REA_CAPTURE_DECODER_OLD_GENERATION_MIB` where REA starts (default 192 MiB, max 16384 MiB).
- **Recorded packet captures (PCAP / PCAPNG):**
  - MCP tool: `inspect_pcap_capture` with `detail: "summary"` for address/DNS/SNI/protocol overviews, or `inspect_packet_capture` (preceded by `inspect_packet_fields`) for TShark display filters and field extraction (requires `REA_TSHARK_COMMAND`).
- **Recorded hardware logic/signal traces:**
  - MCP tool: `inspect_signal_decoders` to verify decoder IDs, channels, and options, then `decode_signal_recording` (requires sigrok-cli via `REA_SIGROK_COMMAND`).

## Step 3: Enforce Evidence Discipline

Apply these analytical guardrails to every finding:

1. **Separate Static Facts from Runtime Observation:**
   - Clearly label whether a claim comes from static binary/archive structure (`open_binary`, AST, decompilation) or observed execution (`capture_browser_scenario`, packet capture, dynamic trace).
   - Presence of a string, symbol, protocol field registration, or code path in a binary does not prove it executes at runtime.

2. **Preserve Artifact Identity and Digests:**
   - Track and cite exact SHA-256 digests, member paths, record ordinals, and memory/file offsets across paginated views so findings remain reproducible.

3. **Distinguish Reconstructions from Original Source:**
   - Decompiled C pseudocode, JADX Java output, and decoded WAT are lossy reconstructions. Highlight compiler artifacts, inlined functions, stripped symbols, and type-recovery limitations when guiding a code reconstruction.

4. **Surface Unknowns Explicitly:**
   - When pagination is truncated, symbols are stripped, fuses block inspection, or capture bodies are omitted, state the exact boundary of what was observed versus what remains unverified.

## Gotchas

- Never instruct the user to blindly re-install Node, Ghidra, JADX, or Wireshark to fix a missing MCP tool; diagnose whether the blocker is client registration, environment variables (`REA_TSHARK_COMMAND`, `REA_WABT_BIN_DIRECTORY`, `REA_SIGROK_COMMAND`), or missing local prerequisites.
- Never fetch external URLs found inside a malware sample, binary string table, or historical HAR/PCAP capture unless the user explicitly authorizes active network probing.
- Start with `detail: "summary"` and bounded pagination (`match_limit`, `line_limit`) on large archives, PCAPs, and signal traces to avoid flooding the context window.
