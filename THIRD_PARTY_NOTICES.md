# 第三方许可与版权声明 / Third-Party Notices

本清单说明目前已知的第三方代码、运行时或执行功能来源，以及发布前仍需核验的范围。它不是任何未来安装包的完整依赖清单。逐个二进制的组件清单、动态链接库闭包和许可义务必须按具体 Release 实物扫描；本地私有仓库所述的最终依赖审计尚未提供。

This notice describes currently known third-party code, runtime, and execution-feature sources and the checks still required before release. It is not a complete dependency list for any future installer. Component inventories, dynamic-library closures, and license obligations must be audited against each actual release binary; the final dependency audit referenced for the private repository was not available in the local checkout.

## QEMU 11.1.3

任务书提供的发行信息指出，计划使用 QEMU 11.1.3。QEMU 的源码包含多个按文件或组件适用的许可证；其中由 GNU GPL v2 覆盖的部分须按其条款提供相应源码。不能仅凭“QEMU”这一名称推定最终二进制全部组件使用同一许可证。每次发行必须记录实际构建选项、随包动态库和各自许可证。

The release information supplied with this task identifies QEMU 11.1.3 as a planned component. QEMU source contains multiple file- or component-level licenses; portions covered by GNU GPL version 2 require corresponding source under their terms. The name “QEMU” alone does not establish one license for every part of a final binary. Each release must record its actual build options, bundled dynamic libraries, and their licenses.

- Upstream source: [QEMU 11.1.3](https://download.qemu.org/qemu-11.1.3.tar.xz)
- Upstream project and licensing details: [QEMU download page](https://www.qemu.org/download/) and [QEMU license documentation](https://www.qemu.org/docs/master/about/license.html)
- Planned corresponding Release source asset: `qemu-11.1.3.tar.xz` (not yet attached; see [SOURCES.md](SOURCES.md)).

## SeaBIOS firmware

任务书将实际使用的 `bios-256k.bin` 对应到 SeaBIOS commit `b52ca86e094d19b58e2304417787e96b940e39c6`，并说明 firmware SHA-256 为 `ae6f6aa973aaccc143f57aa960fb035fd9de4daee4ad0cd713322f8c259e7650`。任务书指定其适用许可证为 GPL-3.0-or-later。正式发布前，必须从实际 Runtime 中重新提取/校验该固件、核对源码提交及原始许可证和版权声明，并在对应 Release 附上源码；当前尚无可核对的 Runtime 或源码归档。

The task brief maps the `bios-256k.bin` in use to SeaBIOS commit `b52ca86e094d19b58e2304417787e96b940e39c6` and gives firmware SHA-256 `ae6f6aa973aaccc143f57aa960fb035fd9de4daee4ad0cd713322f8c259e7650`. The brief identifies GPL-3.0-or-later as the applicable license. Before release, re-extract/check the firmware from the actual Runtime, verify its source commit and original license/copyright notices, and attach the source to the matching Release. No Runtime or source archive is currently available here for verification.

- Upstream source: [SeaBIOS commit](https://github.com/coreboot/seabios/tree/b52ca86e094d19b58e2304417787e96b940e39c6)
- Planned corresponding Release source asset: `seabios-b52ca86e094d19b58e2304417787e96b940e39c6.tar.gz` (not yet attached; see [SOURCES.md](SOURCES.md)).

## Nanos

Nanos is an execution-environment component named in the supplied license materials. The task brief identifies Nanos as Apache-2.0. Whether a specific Release bundles Nanos artifacts, and which exact version/source and notices apply, must be confirmed against that Release's build manifest. This notice does not claim any Nanos binary or source is currently shipped here.

Nanos is an execution-environment component named in the supplied license materials. The task brief identifies Nanos as Apache-2.0. Whether a particular Release bundles Nanos artifacts, and the exact version/source and notices that apply, must be confirmed against that Release's build manifest. This notice does not claim that any Nanos binary or source is currently shipped here.

## OpenAI Codex Seatbelt policy references

私有开发仓库的第三方声明记录：其中 macOS Seatbelt policy 文件改编自 OpenAI Codex `codex-rs/sandboxing/src/`，上游项目采用 Apache-2.0。含有这些改编内容的发行必须保留相应版权/署名及 Apache 许可文本。完整 Apache License 2.0 文本随本仓库提供：[licenses/Apache-2.0.txt](licenses/Apache-2.0.txt)。

The private development repository's third-party notice records that its macOS Seatbelt policy files are adapted from OpenAI Codex's `codex-rs/sandboxing/src/`, whose project license is Apache-2.0. Any release containing those adaptations must preserve applicable copyright/attribution and the Apache license text. The complete Apache License 2.0 text is included at [licenses/Apache-2.0.txt](licenses/Apache-2.0.txt).

- Upstream source: [OpenAI Codex sandboxing sources](https://github.com/openai/codex/tree/main/codex-rs/sandboxing/src)

## Microsoft HCU / MXC and other dependencies

HCU and MXC are identified in the supplied EULA as Microsoft components. Their exact package versions, distribution status, notices, and governing terms must be read from the selected packages and verified for each platform. Do not treat them as GPL components or assume their terms from this summary. The private repository's final Windows HCU and native dependency scans were not available in the checkout used for this documentation.

HCU and MXC are identified in the supplied EULA as Microsoft components. Their exact package versions, distribution status, notices, and governing terms must be read from the selected packages and verified for each platform. Do not classify them as GPL components or infer their terms from this summary. The private repository's final Windows HCU and native-dependency scans were not available in the checkout used for this documentation.

## 交付边界 / Distribution boundary

第三方许可只覆盖其各自组件。`LICENSE.txt` 与 `EULA.txt` 授权的是圳自有专有部分，不取代第三方许可证；必要的第三方源码获取、修改和再分发权利仍由相应上游许可决定。详见 [SOURCES.md](SOURCES.md) 中的逐版本 GPL 源码交付要求。

Third-party licenses apply to their respective components. `LICENSE.txt` and `EULA.txt` cover Zhen-owned proprietary portions and do not replace third-party licenses; source, modification, and redistribution rights required by an upstream license remain governed by that license. See the per-release GPL source requirements in [SOURCES.md](SOURCES.md).
