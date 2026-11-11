# GPL 源码与构建材料 / GPL Sources and Build Materials

## 当前交付状态 / Current delivery status

截至本文件提交时，`AlbertYang514/zhen-releases` 尚无 GitHub Release、Runtime Bundle 或 Release 附件。因此，本文件列出的源码包均为**待交付项**，不能据此认定任何 GPL 源码义务已经完成。源码包应放在与对应 Runtime 二进制相同的 GitHub Release/tag 中，不应提交到 `main` 的 Git 历史。

As of this document, `AlbertYang514/zhen-releases` has no GitHub Release, Runtime Bundle, or Release assets. Every source archive below is **pending delivery**; this document does not claim that any GPL source obligation has been fulfilled. Source archives must be attached to the same GitHub Release/tag as the corresponding Runtime binary, not committed to the `main` Git history.

## 预期源码材料 / Expected source materials

以下名称和版本来自本次任务书提供的发行信息。指定的私有 `runtime-sources.lock.json`、许可证审计记录、打包 workflow 和源码归档未能在当前本地私有仓库检出中找到，故发布前必须从实际 release staging 重新核实内容、哈希、修改和对应二进制。上游下载地址可用于获取原始源码，**不能替代本项目实际构建所对应的 Release 源码附件**。

The names and versions below come from the release information supplied with this task. The named private `runtime-sources.lock.json`, license audit, packaging workflow, and source archives were not present in the local private checkout. Before release, verify the staged contents, hashes, modifications, and binary correspondence. Upstream download links help locate upstream source; **they do not replace the Release source archives corresponding to the actual build**.

| Release asset name | Component and applicable license | Source identity / expected verification | Status |
| --- | --- | --- | --- |
| `qemu-11.1.3.tar.xz` | QEMU 11.1.3 source; QEMU files have their applicable upstream licenses, including GPL-2.0-covered parts. Preserve the upstream license and notices; audit the exact configured build and linked libraries. | Upstream archive: [download.qemu.org/qemu-11.1.3.tar.xz](https://download.qemu.org/qemu-11.1.3.tar.xz). The task brief gives expected SHA-256 `5a8289c7940dc77d3873c81a6c29ba3afe1446fc9d36e77fa1658cdbecb84b22`; not independently checked against a staged archive. | Not present; not delivered |
| `seabios-b52ca86e094d19b58e2304417787e96b940e39c6.tar.gz` | SeaBIOS source at commit `b52ca86e094d19b58e2304417787e96b940e39c6`; task brief identifies the bundled ROM as GPL-3.0-or-later. Include the original license and copyright notices. | Upstream source: [coreboot/seabios commit](https://github.com/coreboot/seabios/tree/b52ca86e094d19b58e2304417787e96b940e39c6). The task brief associates this source with `bios-256k.bin` SHA-256 `ae6f6aa973aaccc143f57aa960fb035fd9de4daee4ad0cd713322f8c259e7650`; archive hash and binary match remain unverified here. | Not present; not delivered |
| `zhen-qemu-build-material-11.1.3.tar.gz` | Publicly releasable build scripts, patches, source lock, configuration, and instructions required to rebuild the corresponding QEMU/firmware artifacts. Include all changes to covered source and applicable build/install control scripts. | Must be generated from the actual release staging inputs. Must not include Zhen Core, CLI, Desktop, App Server, Guest/Bootstrap proprietary source, credentials, or internal debugging data. | Not present; not delivered |

### 每个 Release 的交付方式 / Per-release delivery

1. Attach all applicable source archives and build materials to the **same tag and Release** as the Runtime Bundle they cover.
2. Preserve upstream copyright and license text. Add every Zhen-authored patch and the build/install control scripts required to reproduce the covered binaries.
3. Publish an archive SHA-256 for each attached file and compare it with the uploaded asset. Record the covered binary file names and hashes from that Release's `SHA256SUMS`.
4. Verify source-to-binary correspondence, source versions, patches, configured components, and the final linked-library closure before describing an obligation as satisfied.
5. QEMU source, SeaBIOS firmware source, and build materials are separate deliverables. A license notice, upstream link, or source archive for one component does not substitute for another required artifact.

1. Attach each applicable source archive and build-material archive to the **same tag and Release** as the Runtime Bundle it covers.
2. Preserve upstream copyright and license text. Include all Zhen-authored patches and the build/install control scripts required to reproduce covered binaries.
3. Publish and verify a SHA-256 for every attached archive. Record the covered binary file names and hashes from that Release's `SHA256SUMS`.
4. Verify source-to-binary correspondence, versions, patches, configured components, and the final linked-library closure before stating that an obligation has been met.
5. QEMU source, SeaBIOS firmware source, and build materials are separate deliverables. A notice, upstream link, or one component's archive cannot replace another required artifact.

After a Release exists, the asset URL form is `https://github.com/AlbertYang514/zhen-releases/releases/download/<tag>/<asset-name>`. Replace `<tag>` only with the actual published tag. No tag-specific links are provided while no Release exists.

Release-specific download form (after publication): `https://github.com/AlbertYang514/zhen-releases/releases/download/<tag>/<asset-name>`. Replace `<tag>` with the actual published tag. No tag-specific links are listed before a Release exists.

## 专有源码边界 / Proprietary source boundary

源码交付只覆盖适用开源许可证要求公开的第三方组件与必要构建材料。不得将私有开发仓库中的 Zhen Core、CLI、Desktop、App Server、Guest、Bootstrap 或其他自有实现源码放入 Release 附件。第三方 GPL 源码公开不改变这些 Zhen 自有部分的专有许可。

Source delivery is limited to third-party components and required build materials covered by applicable open-source obligations. Do not place Zhen Core, CLI, Desktop, App Server, Guest, Bootstrap, or other proprietary implementation source from the private development repository in Release assets. Publishing third-party GPL source does not change the proprietary license of Zhen-owned software.
