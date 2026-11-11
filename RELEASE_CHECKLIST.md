# 首版 Beta 发布检查 / First Beta Release Checklist

此清单须针对每个候选 Release 逐项记录证据。状态仅在检查完成并保存证据后标记 PASS；当前各项均为 **NOT RUN / 待完成**。不要创建空 Release，也不要发布未验收的二进制。

Use this checklist for every release candidate and record evidence for each item. Mark PASS only after completing the check and retaining evidence. All items are currently **NOT RUN / pending**. Do not create an empty Release or publish unaccepted binaries.

## 构建与平台 / Build and platform

- [ ] 确认本次实际产物的平台和架构：macOS arm64、Windows x64、Linux x86_64；只列出真正构建并准备发布的目标。
- [ ] 在目标 OS/架构上安装并启动 CLI/Desktop 实际产物，记录版本、步骤、日志和结果；未验收的平台明确标为未验证。
- [ ] 区分 CLI 可执行文件、Desktop 安装器和 Runtime Bundle；不将 Runtime Bundle 标为 Desktop 安装器。
- [ ] 记录签名、公证、安装器警告等真实状态；没有验证证据时不作声明。
- [ ] 检查安装、升级/覆盖、卸载、首次配置、API Key 读取与错误处理。

- [ ] Record actual artifact platforms and architectures: macOS arm64, Windows x64, and Linux x86_64. List only targets built and intended for this release.
- [ ] Install and launch the actual CLI/Desktop artifact on its target OS/architecture. Record version, steps, logs, and result; label untested platforms clearly.
- [ ] Distinguish CLI executables, Desktop installers, and Runtime Bundles. Do not describe a Runtime Bundle as a Desktop installer.
- [ ] Record actual signing, notarization, and installer-warning status. Make no claim without evidence.
- [ ] Check install, upgrade/overwrite, uninstall, first-run configuration, API key handling, and error paths.

## 法律文件与第三方组件 / Legal files and third-party components

- [ ] 将 `LICENSE.txt`、`EULA.txt`、`THIRD_PARTY_NOTICES.md` 及所需第三方许可原文纳入每个适用安装包/发行资产。
- [ ] 核对中英文本、版权年份 © 2026 Albert Yang，以及自有专有部分与第三方许可的边界。
- [ ] 按平台和产物扫描最终依赖闭包，覆盖 QEMU 动态库、firmware、Windows HCU/MXC 以及实际随包分发的其他组件；记录每项版本、来源、许可证和 notice。
- [ ] 确认 Apache-2.0 Seatbelt 来源署名与许可证在包含相应改编内容的发行中完整保留。
- [ ] 未完成的 Windows/Linux 原生依赖审计、HCU/MXC 许可核验或其他未决项保持 NOT RUN；不可用已有 Notice 替代审计。

- [ ] Include `LICENSE.txt`, `EULA.txt`, `THIRD_PARTY_NOTICES.md`, and required third-party license texts in every applicable installer/release asset.
- [ ] Review bilingual text, Copyright © 2026 Albert Yang, and the boundary between proprietary Zhen-owned software and third-party licenses.
- [ ] Scan final dependency closure for each platform/artifact, including QEMU dynamic libraries, firmware, Windows HCU/MXC, and every other bundled component. Record versions, sources, licenses, and notices.
- [ ] Preserve Apache-2.0 Seatbelt attribution and license in releases that include the corresponding adaptations.
- [ ] Keep incomplete Windows/Linux native-dependency audits, HCU/MXC license checks, and other open items NOT RUN; existing notices are not a substitute for the audit.

## GPL 源码与校验 / GPL source and integrity

- [ ] `qemu-11.1.3.tar.xz` 与本次 Runtime 二进制所用 QEMU 版本、配置和补丁对应；保留 QEMU 原始版权、许可文本及必要源文件。
- [ ] `seabios-b52ca86e094d19b58e2304417787e96b940e39c6.tar.gz` 对应实际固件；校验 `bios-256k.bin` SHA-256，并保留 SeaBIOS 原始许可与版权声明。
- [ ] `zhen-qemu-build-material-11.1.3.tar.gz` 包含实际构建脚本、所有相关 patches、source lock、配置、必要构建/安装控制脚本及可复现说明；人工审查确认未包含 Zhen 专有实现、凭据或内部调试资料。
- [ ] 对每个 GPL 源码包、构建材料和二进制计算 SHA-256；复核 `SHA256SUMS` 与实际上传资产完全一致。
- [ ] 在干净环境中按公开材料重新构建/验证所覆盖产物，并核对其与 Release 二进制的版本、构建选项和补丁对应关系。
- [ ] 将适用源码归档与构建材料上传到覆盖该 Runtime 的同一 GitHub Release/tag；从公开下载链接实际下载后再验证校验和。
- [ ] 未真实上传、可下载和核验前，README、`SOURCES.md`、Notice 和发布说明都不得表示源码已交付或 GPL 义务已履行。

- [ ] Confirm `qemu-11.1.3.tar.xz` matches the QEMU version, configuration, and patches used by this Runtime binary; preserve original QEMU copyright, license text, and necessary source files.
- [ ] Confirm `seabios-b52ca86e094d19b58e2304417787e96b940e39c6.tar.gz` corresponds to the actual firmware; verify the `bios-256k.bin` SHA-256 and preserve original SeaBIOS license and copyright notices.
- [ ] Ensure `zhen-qemu-build-material-11.1.3.tar.gz` contains actual build scripts, every relevant patch, source lock, configuration, required build/install control scripts, and reproducible instructions. Manually confirm it contains no proprietary Zhen implementation, credentials, or internal debugging data.
- [ ] Compute SHA-256 for every GPL source archive, build-material archive, and binary. Confirm `SHA256SUMS` exactly matches the uploaded assets.
- [ ] Rebuild/verify the covered artifacts from the public materials in a clean environment and confirm version, build options, and patches correspond to the Release binaries.
- [ ] Upload applicable source archives and build materials to the same GitHub Release/tag as the Runtime they cover. Download them from the public URLs and verify checksums.
- [ ] Until files have actually been uploaded, are downloadable, and have been verified, README, `SOURCES.md`, notices, and release notes must not claim source delivery or GPL compliance is complete.

## 发布与反馈 / Publication and feedback

- [ ] 核对 Release assets 清单、文件名、平台标签、版本/tag 和 `SHA256SUMS`。
- [ ] 核对 README、`SOURCES.md` 与 Release 资产链接能打开且指向本次版本；不存在的资产不添加链接。
- [ ] 确认 Issue/反馈渠道提醒用户移除 API Key、敏感代码、日志和个人信息。
- [ ] 完成 Git diff、文档链接和专有源码边界检查；只公开必要的许可与第三方源码材料。

- [ ] Verify the Release asset list, file names, platform labels, version/tag, and `SHA256SUMS`.
- [ ] Verify README, `SOURCES.md`, and Release asset links open and point to this version; do not link to nonexistent assets.
- [ ] Confirm the Issue/feedback channel asks users to remove API keys, sensitive code, logs, and personal information.
- [ ] Review the Git diff, documentation links, and proprietary-source boundary. Publish only required license materials and third-party source.
