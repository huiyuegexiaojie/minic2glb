# 第三方组件声明

Minic2GLB 分发包装载了以下第三方组件。这些组件按各自许可分发，其许可条款就相应组件而言优先于本项目的主许可（见 [LICENSE](LICENSE)）。

本文件列出组件、版本、许可与合规说明。版本以 `minic2glb-v1.0.0-win64.zip` 内 `runtime-versions.txt` 为准。

---

## 1. Python 运行时与库

| 组件 | 打包版本 | 许可 | 说明 |
|---|---|---|---|
| Python | 3.14.0 | PSF License Agreement | 解释器与标准库，包内 `licenses/Python-LICENSE.txt` |
| PySide6 | 6.11.1 | **LGPL-3.0-only**（或 GPL / 商业） | 见第 3 节 |
| PySide6_Essentials | 6.11.1 | **LGPL-3.0-only** | 见第 3 节 |
| shiboken6 | 6.11.1 | **LGPL-3.0-only** | 见第 3 节 |
| Qt 6 运行库 | 随 PySide6 6.11.1 | **LGPL-3.0** | 见第 3 节 |
| numpy | 2.4.4 | BSD-3-Clause | 数值计算 |
| Pillow | 12.2.0 | MIT-CMU (HPND) | 图像解码与编码 |
| lz4 (python binding) | 4.4.5 | BSD-2-Clause | 资源包块解压 |
| texture2ddecoder | 1.0.6 | MIT | BC7 等压缩纹理解码 |
| PyInstaller | 6.19.0 | GPL-2.0 + Bootloader Exception | 打包工具，其引导程序随包 |

## 2. 原生库（随 `_internal` 目录分发）

许可原文已随包收录于 `licenses/native/`，对应关系见 `licenses/NATIVE-DLLS-INDEX.txt`。

| 文件 | 组件 | 许可 | 许可文件 |
|---|---|---|---|
| `libarchive-13.dll` | libarchive | BSD-2-Clause | `native/BSD-AND-CC0-LIBS.txt` |
| `libbz2-1.dll` | bzip2 | bzip2 License | `native/bzip2-LICENSE.txt` |
| `libb2-1.dll` | libb2 (BLAKE2) | CC0-1.0 | `native/BSD-AND-CC0-LIBS.txt` |
| `libcrypto-3-x64.dll` | OpenSSL 3 | Apache-2.0 | `native/openssl-LICENSE.txt` |
| `liblz4.dll` | LZ4 | BSD-2-Clause | `native/BSD-AND-CC0-LIBS.txt` |
| `liblzma-5.dll` | XZ Utils | 0BSD / Public Domain | `native/xz-COPYING.txt` |
| `libzstd.dll` | Zstandard | BSD-3-Clause | `native/zstd-LICENSE.txt` |
| `zlib1.dll` | zlib | zlib License | `native/zlib-LICENSE.txt` |
| `libiconv-2.dll` | GNU libiconv | **LGPL-2.1** | `native/libiconv-COPYING.LIB.txt` |
| `libwinpthread-1.dll` | mingw-w64 | BSD-3-Clause | `native/libwinpthread-COPYING.txt` |
| `libgcc_s_seh-1.dll` | GCC 运行时 | **GPL-3.0 + Runtime Exception** | 见第 4 节 |
| `libstdc++-6.dll` | GCC 运行时 | **GPL-3.0 + Runtime Exception** | 见第 4 节 |

---

## 3. LGPL 合规说明（PySide6 / Qt / libiconv）

本项目以**闭源**方式分发，涉及 LGPL 组件时的处理如下。这些要求是强制的，缺少任一项都可能构成许可违约。

**3.1 动态链接，不静态合并**

Qt 库、libiconv 均以独立动态库（`.dll` / `.pyd`）形式随包分发，未与主程序静态链接，未做符号隐藏或重命名。

**3.2 保持可替换性（关键）**

发行包采用**目录式布局（onedir）**，Qt 库以独立文件存在于可定位目录中，用户可直接替换为自行编译的兼容版本。

**不得**使用"单文件自解压（onefile）"方式打包。onefile 会把 Qt 库解压到临时目录并在退出时删除，导致用户无法替换，通常被认为违反 LGPL 第 4 条（结合安装信息的要求）。

**3.3 提供许可文本**

发行包内已包含以下文件：

```
licenses/
├── PySide6/
│   ├── LICENSE.LGPLv3              LGPL v3 全文
│   └── LicenseRef-Qt-Commercial.txt Qt 官方许可引用
├── PySide6_Essentials/
│   └── LICENSE.LGPLv3
├── shiboken6/
│   ├── LICENSE.LGPLv3
│   └── LicenseRef-Qt-Commercial.txt
├── QT-LGPL-COMPLIANCE.txt          动态链接、可替换性、源码获取说明
├── Python-LICENSE.txt
├── native/                         12 个原生库的许可原文
└── NATIVE-DLLS-INDEX.txt           DLL → 许可文件对照表
```

注意：`LicenseRef-Qt-Commercial.txt` 只是 Qt 商业许可的引用说明，**不能替代 LGPL 全文**。上一版发行包只有该文件而缺少 `LICENSE.LGPLv3`，属于合规缺口，本版已补齐。

**3.4 提供源码获取途径**

本项目**未修改** Qt、PySide6 与 libiconv 的任何源码。其对应源码可从上游获取：

- Qt 6：<https://download.qt.io/official_releases/qt/>
- Qt for Python (PySide6)：<https://download.qt.io/official_releases/QtForPython/>
- GNU libiconv：<https://www.gnu.org/software/libiconv/>

若日后修改了上述任一组件的源码，必须以 LGPL 允许的方式公开修改后的源码。

**3.5 声明**

发行包内的 `licenses/` 目录与"关于"对话框须写明：

> 本软件使用 Qt / PySide6（LGPL v3）与 GNU libiconv（LGPL v2.1）。这些组件以动态链接方式使用，可被替换。源码可从上游项目获取。

---

## 4. GCC 运行库说明

`libgcc_s_seh-1.dll` 与 `libstdc++-6.dll` 以 GPL-3.0 授权，但附带 **GCC Runtime Library Exception**。该例外条款明确允许将这两个运行库与独立程序一同分发，且不要求该独立程序以 GPL 授权。

因此本项目作为闭源软件随包分发这两个 DLL 是允许的，前提是：

- 不修改运行库本身；
- 随附 GPL-3.0 与例外条款文本。本包在 `licenses/native/NATIVE-DLLS-INDEX.txt` 中给出版权说明与官方获取地址，GPL-3.0 与例外条款全文可自 GCC 官方网站取得。

---

## 5. Apache-2.0 说明（OpenSSL）

OpenSSL 3 以 Apache-2.0 授权。分发时须：

- 保留其版权、专利、商标与归属声明；
- 许可全文已随包收录于 `licenses/native/openssl-LICENSE.txt`；
- 不得暗示获得 OpenSSL 项目背书。

---

## 6. 随包内容的边界

**不包含**：

| 内容 | 原因 |
|---|---|
| 游戏的模型、贴图、音效 | 属第三方著作权内容 |
| 从游戏安装包提取的实体模型与贴图（v1.0.0 已移除 178 个 `asset-overrides` 文件） | 同上 |
| 用户存档与导出的 GLB | 属用户数据 |
| 本项目源代码（`.py` / `.pdb` / 工程文件） | 闭源发布 |

**仍包含，且风险已披露**：

| 内容 | 用途 | 说明 |
|---|---|---|
| `_internal/map-native/config/*.csv|xml|lua` | 方块定义、群系表、模型表、绘制 XML、色表 | 属游戏数据。程序在可读取本机游戏资源时会优先使用本机的最新配置，内置文件作为兜底 |
| `_internal/**/chunk.zdict` | 解压存档所需的 64 KB 字典 | 属游戏数据。缺失时无法读取任何存档 |

以上两项是软件离线可用的前提，移除会显著降低可用性。此风险由发布者自行承担并在此明示。

软件运行时会读取用户本机已安装的游戏资源包，但**不复制、不修改、不重新分发**其中的任何文件。

---

## 7. 发布前检查清单

v1.0.0 实测状态：

- [x] 打包产物为 onedir 布局，Qt DLL 为独立文件（`_internal/PySide6/`）
- [x] `licenses/` 包含 LGPLv3 全文、Qt 许可引用与原生库许可原文
- [x] `licenses/NATIVE-DLLS-INDEX.txt` 给出 12 个 DLL 与许可文件的对应关系
- [x] `licenses/QT-LGPL-COMPLIANCE.txt` 说明动态链接与可替换性
- [x] 包内不含 `.py`、`.pdb`、`.blockmesh`、`.obj`、`.pyi`
- [x] 包内不含 `asset-overrides` 美术资源（178 个文件已移除）
- [x] `distribution-manifest.json` 已按最终文件重新生成（464 条 SHA-256）
- [ ] "关于"对话框尚未显示 LGPL/Qt 声明与源码获取链接（下一版补）
- [ ] 包内仍含 `map-native/config/` 的方块定义与绘制配置（**游戏数据，风险已披露**）
- [ ] 包内仍含 `chunk.zdict` 压缩字典（解压存档必需，**游戏数据，风险已披露**）
- [ ] 程序存在启动/导出时的联网启用检查，已写入 README 披露

---

最后更新：2026-11-17
