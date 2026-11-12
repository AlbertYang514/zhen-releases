# Zhen Harness

**本地优先的 Agent 执行环境 / A local-first agent execution environment**

> 下载 0.1.0-beta.1 / Download 0.1.0-beta.1:
> https://github.com/AlbertYang514/zhen-releases/releases/tag/v0.1.0-beta.1
>
> 官网 / Official website:
> https://albert.gd.cn/zhen
>
> 本仓库只发布二进制、Runtime 和第三方源码。Zhen Harness 的源代码仓库不公开。

---

# 中文

## 这是什么

Zhen Harness 是一个本地优先的桌面 Agent 执行环境。它把“模型”和“实际执行”分开：

- **Workspace**：你本机上的一个真实目录
- **Target**：执行位置，可以是本机，也可以是一台远程机器
- **Runner / Sandbox**：真正运行命令的受信任执行链

模型只负责生成意图，命令由 Target 上的受保护 Runner 执行。Zhen 不会因为模型输出就静默提升权限；所有执行都必须先完成 Setup 和 Verify。

## 当前版本

**0.1.0-beta.1**

| 平台 | 文件 | 要求 |
| --- | --- | --- |
| macOS | `Zhen Harness_0.1.0-beta.1_aarch64.dmg` | macOS 11+，Apple Silicon |
| Windows | `Zhen Harness_0.1.0-beta.1_x64-setup.exe` | Windows 10/11 x64，**需要管理员权限** |
| Linux | `Zhen Harness_0.1.0-beta.1_amd64.deb` | glibc ≥ 2.38（Ubuntu 24.04） |

## 安装

```bash
# macOS
open "Zhen Harness_0.1.0-beta.1_aarch64.dmg"
# 把 App 拖进 Applications；首次打开请右键 → 打开

# Linux
sudo apt install "./Zhen Harness_0.1.0-beta.1_amd64.deb"

# Windows
# 双击安装包，允许 UAC。应用每次启动都会请求管理员权限
```

命令行（可选）：

```text
zhen-cli-macos-aarch64
zhen-cli-windows-x86_64.exe
zhen-cli-linux-x86_64
```

## 安装包里有什么

- QEMU 11.1.3（aarch64 / x86_64）
- Nanos Guest 与源码构建 Kernel
- 五个 Remote Bootstrap（Linux x86_64/arm64、macOS x86_64/arm64、Windows x64）
- Microsoft MXC 1.0.1 运行时（三端都带，连接 Windows Target 时需要）
- Microsoft HCU v1.0.0（macOS / Windows；Linux 不包含）
- 完整第三方许可证与来源记录

## 许可证与对应源码

Zhen Harness 自有部分是专有软件。第三方组件按各自许可证分发，许可证文本随安装包提供：

```text
macOS / Linux Runtime:  share/licenses/
Desktop 安装包:          resources/licenses/
```

GPL / LGPL 对应源码与本版本同 tag 发布：

- `qemu-11.1.3.tar.xz`
- `seabios-b52ca86….tar.gz`
- `zhen-qemu-build-material-11.1.3.tar.gz`
- 14 个 LGPL 库源码包（glib、util-linux、gmp、gnutls、nettle、libidn2、libunistring、libtasn1、gettext、libiconv、libssh、libusb）

映射关系见 `THIRD-PARTY-SOURCES.md`。动态库以独立文件分发，用户可以替换。

## 校验

```bash
sha256sum -c SHA256SUMS
```

```text
DMG  28bd167aaf9c68e0134c1043ffece947a92b995f71c9194640df199a0f4c3780
DEB  344b3c2e054d760027305e02c00b4b01fcb3cc52c0ffe5215d6602e084c0dffb
EXE  fa459daaee10af03aa0d82339adf1e58b73f3bc48a2b93711d60b084c16f6996
```

---

## 常见问题（中文）

### 1. 为什么 Setup 完成后，某些会话里还是不能执行？

Setup 和 Verify 是**机器级**状态；执行能力是**会话级**授权。

- 已经开始的会话不会自动获得新 Target 或新权限
- 新加 Workspace、新注册 Target 后，请**新建一个会话**
- 如果某个会话被 revoke 过，它不会自动恢复，需要重新授权

### 2. 为什么在某个会话之后，Workspace 不能热添加？

当前设计里，会话在创建时绑定 Target 和权限位。运行中的会话不会热加载新的 Workspace，这是有意的 fail-closed 行为。请结束当前会话后新建。

### 3. Windows 上会弹出终端窗口吗？

旧版本会在执行、验证、修改权限时弹出 PowerShell/控制台窗口。0.1.0-beta.1 已统一使用 `CREATE_NO_WINDOW`，正常路径不再弹窗。

如果仍然出现：

- 确认安装的是本页 `0.1.0-beta.1` 安装包
- 某些第三方程序（例如自带的 OpenSSH 工具）可能仍会短暂闪窗
- 把具体操作路径反馈给我们

### 4. 为什么 Windows 版必须用管理员权限运行？

Windows Target 的 Runner、HMAC key、SSH key 的 ACL 只允许 `SYSTEM` 和 `Administrators` 读取。普通权限进程读不到这些受保护证据，Verify 会失败，执行链会 fail-closed，不会降级到普通 shell。

因此：

- 应用启动时会请求 UAC
- 拒绝 UAC 后执行功能不可用，这是预期行为

### 5. Linux 执行为什么偶尔很慢？

已知问题。表现为：

- 大多数命令很快
- 偶尔一次执行会卡住一段时间
- 极端情况下出现“几乎瞬时完成但结果不完整”的情况

这**不影响 macOS → Linux Target** 的路径。我们仍在定位，属于 Beta 已知问题，不是权限或沙箱绕过。

### 6. Windows 验证为什么偶尔要十几秒？

Verify 会做以下检查，其中部分调用 Windows 计划任务和 CIM 接口，单次约 0.5–1 秒：

- Runner / config / HMAC key 的哈希
- sshd 配置、Host Key、authorized_keys 绑定
- 计划任务身份与交互会话
- Runner 进程所有者 SID

累计 10–15 秒属于当前实现的正常范围，不是卡死。

### 7. macOS 版本稳定吗？

三平台里 macOS 是测试最充分的，相对最稳定。已知问题主要在 Linux 执行延迟和 Windows 的权限/计划任务路径。

### 8. HCU 为什么会抢鼠标键盘？

HCU（Computer Use）通过真实桌面输入工作。它运行时：

- 会移动鼠标、发送键盘事件
- 可能抢占当前前台窗口

建议：

- 运行 HCU 时不要同时操作鼠标键盘
- 用完停止 HCU 会话
- 需要并行工作时，用另一台机器或另一个桌面会话

### 9. 为什么我的 Workspace 被拒绝，说包含硬链接？

这是一个 fail-closed 检查：ACL 是路径权限，硬链接会让同一 inode 出现在 Workspace 之外的路径上。

检查：

```bash
find /path/to/workspace -xdev -type f -links +1 | head
```

解决办法：换一个普通项目目录，或先移除硬链接。NVM、缓存目录、复制来的 `node_modules`、备份目录是常见来源。

### 10. 注册远程 Ubuntu 时提示缺少 Bootstrap？

安装包的 `resources/bin` 必须包含五个 Bootstrap。0.1.0-beta.1 已包含全部五个：

```text
zhen-bootstrap-linux-aarch64
zhen-bootstrap-linux-x86_64
zhen-bootstrap-macos-aarch64
zhen-bootstrap-macos-x86_64
zhen-bootstrap-windows-x86_64.exe
```

如果仍报错，请确认没有被旧版本覆盖，并检查安装目录。

### 11. 连接 Windows Target 为什么需要 MXC？

MXC 是 Windows 侧的进程容器运行时。Host 会把 MXC payload 安装到 Windows Target，因此**三端安装包都带 MXC**，包括 macOS 和 Linux。

### 12. 安装包为什么会被系统拦截？

Beta 没有代码签名：

- macOS：右键 → 打开，或在“隐私与安全性”里允许
- Windows：SmartScreen → 更多信息 → 仍要运行

---

# English

## What this is

Zhen Harness is a local-first desktop agent execution environment. It separates model output from real execution:

- **Workspace**: a real directory on your machine
- **Target**: where commands run, locally or on a remote machine
- **Runner / Sandbox**: the trusted execution chain

The model produces intent; commands run through a protected Runner on the Target. Nothing is silently escalated, and execution is available only after Setup and Verify succeed.

## Current release

**0.1.0-beta.1**

| Platform | File | Requirements |
| --- | --- | --- |
| macOS | `Zhen Harness_0.1.0-beta.1_aarch64.dmg` | macOS 11+, Apple Silicon |
| Windows | `Zhen Harness_0.1.0-beta.1_x64-setup.exe` | Windows 10/11 x64, **administrator required** |
| Linux | `Zhen Harness_0.1.0-beta.1_amd64.deb` | glibc ≥ 2.38 (Ubuntu 24.04) |

## Install

```bash
# macOS
open "Zhen Harness_0.1.0-beta.1_aarch64.dmg"   # drag to Applications, right-click → Open

# Linux
sudo apt install "./Zhen Harness_0.1.0-beta.1_amd64.deb"

# Windows
# Run the installer and accept the UAC prompt. The app requests elevation on every launch.
```

Optional CLIs: `zhen-cli-macos-aarch64`, `zhen-cli-windows-x86_64.exe`, `zhen-cli-linux-x86_64`.

## What is bundled

- QEMU 11.1.3 system emulators (aarch64 / x86_64)
- Nanos Guest and a source-built kernel
- Five remote Bootstraps (Linux x86_64/arm64, macOS x86_64/arm64, Windows x64)
- Microsoft MXC 1.0.1 runtime (all platforms, required for Windows Targets)
- Microsoft HCU v1.0.0 (macOS / Windows only)
- Complete third-party licenses and provenance

## Licensing and corresponding source

Zhen Harness itself is proprietary. Third-party components ship under their own terms.
License material is inside every package:

```text
Runtime: share/licenses/
Desktop: resources/licenses/
```

Corresponding source is published under the same tag:

- `qemu-11.1.3.tar.xz`
- `seabios-b52ca86….tar.gz`
- `zhen-qemu-build-material-11.1.3.tar.gz`
- 14 LGPL library source archives (glib, util-linux, gmp, gnutls, nettle, libidn2, libunistring, libtasn1, gettext, libiconv, libssh, libusb)

See `THIRD-PARTY-SOURCES.md` for the exact mapping. Dynamic libraries are shipped as
separate files and can be replaced.

## Verify

```bash
sha256sum -c SHA256SUMS
```

```text
DMG  28bd167aaf9c68e0134c1043ffece947a92b995f71c9194640df199a0f4c3780
DEB  344b3c2e054d760027305e02c00b4b01fcb3cc52c0ffe5215d6602e084c0dffb
EXE  fa459daaee10af03aa0d82339adf1e58b73f3bc48a2b93711d60b084c16f6996
```

---

## FAQ (English)

### 1. Setup succeeded, but execution is still disabled in my session

Setup and Verify are machine-level. Execution permission is per-session.

- Existing sessions do not gain new Targets or new capabilities
- After adding a Workspace or Target, start a new session
- A revoked session stays revoked until re-authorized

### 2. Why can't I hot-add a Workspace after a session starts?

Sessions bind their Target and permission tier at creation. Running sessions do not
reload new Workspaces. This is intentional fail-closed behavior; start a new session.

### 3. Does Windows still flash terminal windows?

Older builds spawned PowerShell and console processes during execution, verification and
ACL changes. 0.1.0-beta.1 sets `CREATE_NO_WINDOW` on every first-party child process.

If you still see a window:

- confirm you installed this build
- some third-party tools (for example bundled OpenSSH binaries) may still flash briefly
- report the exact action you performed

### 4. Why does Windows require administrator rights?

The Windows Target stores its Runner, HMAC key and SSH keys with an ACL that only
`SYSTEM` and `Administrators` can read. A normal token cannot read that evidence, so
Verify would fail and execution stays fail-closed. There is no fallback to a plain
user shell.

The app therefore requests elevation at launch. Declining UAC disables execution by design.

### 5. Why is Linux execution occasionally slow?

Known issue:

- most commands are fast
- occasionally a run stalls for a while
- in rare cases a run reports completion almost instantly with incomplete results

This does not affect macOS → Linux Target. It is a Beta issue under investigation, not a
sandbox or permission bypass.

### 6. Why does Windows Verify sometimes take 10–15 seconds?

Verify hashes the Runner, config and HMAC key, checks the sshd layout and key binding,
inspects the scheduled task identity and session, and resolves each Runner process owner
SID. Some of those Windows calls take 0.5–1 second each, so 10–15 seconds is normal for
this implementation.

### 7. Is macOS more stable?

macOS is the most exercised platform in this Beta. The known rough edges are Linux
execution latency and the Windows elevation/scheduled-task path.

### 8. Why does HCU take over my mouse and keyboard?

HCU drives the real desktop. While it runs it moves the pointer and sends keyboard
events, and it may raise its own window.

Recommendations:

- do not use the mouse or keyboard while HCU runs
- stop the HCU session when finished
- use another machine or desktop session for parallel work

### 9. My Workspace is refused because it contains hard links

This is a deliberate fail-closed check: an ACL grant is path-based, but a hard link is a
second name for the same inode, so the grant could expose the file through a path outside
the Workspace.

```bash
find /path/to/workspace -xdev -type f -links +1 | head
```

Choose a normal project directory, or remove the hard links first. NVM, caches, copied
`node_modules` and backup trees are common sources.

### 10. Adding a remote Ubuntu machine says a Bootstrap is missing

`resources/bin` must contain all five Bootstrap artifacts. 0.1.0-beta.1 includes them.
If the error persists, verify that an older install did not overwrite the directory.

### 11. Why does connecting to a Windows Target need MXC?

MXC is the Windows process-container runtime. The Host installs the MXC payload on the
Windows Target, so every platform package ships it, including macOS and Linux.

### 12. Why does the OS block the installer?

The Beta is unsigned:

- macOS: right-click → Open, or allow it in Privacy & Security
- Windows: SmartScreen → More info → Run anyway
