---
title: Tauri构建时下载Wix和NSIS失败问题
tags:
  - Tauri构建时下载Wix和NSIS失败问题
date: 2025-01-23 11:33:19
categories:
  - Tauri
cover: https://tauri.app/_astro/logo_light.Br3nqH4L.svg
---

## 1.问题

> 在Tauri构建时会从github下载Wix和NSIS工具，因为国内网络原因导致下载失败（貌似使用了代理也没用），解决办法是提前下载这两个工具到本地

```powershell
Running light to produce X:\Tauri\tauri-shop-admin\src-tauri\target\release\bundle\msi\tauri-shop-admin_0.1.0_x64_en-US.msi
    Warn NSIS directory contains mis-hashed files. Redownloading them.
    Downloading https://github.com/tauri-apps/nsis-tauri-utils/releases/download/nsis_tauri_utils-v0.4.1/nsis_tauri_utils.dll
failed to bundle project: `https://github.com/tauri-apps/nsis-tauri-utils/releases/download/nsis_tauri_utils-v0.4.1/nsis_tauri_utils.dll: Connection Failed: Connect error: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。 (os error 10060)`
    Error failed to bundle project: `https://github.com/tauri-apps/nsis-tauri-utils/releases/download/nsis_tauri_utils-v0.4.1/nsis_tauri_utils.dll: Connection Failed: Connect error: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。 (os error 10060)`
 ELIFECYCLE  Command failed with exit code 1.
```

## 2.步骤

### 1.下载Wix工具

前往[Wix发行页面](https://github.com/wixtoolset/wix3/releases),下载[wix314-binaries.zip](https://github.com/wixtoolset/wix3/releases/download/wix3141rtm/wix314-binaries.zip)压缩包，注意版本号

![image-20250123114524165](https://s2.loli.net/2025/01/23/vkoRhZPnWgya9LA.png)

### 2.下载NSIS工具

前往Tauri[资源下载页面](https://github.com/tauri-apps/binary-releases/releases/)，下载[nsis-3.zip](https://github.com/tauri-apps/binary-releases/releases/download/nsis-3/nsis-3.zip)

![image-20250123115002068](https://s2.loli.net/2025/01/23/U1SAftqpmjEon93.png)

另外还需要下载[nsis_tauri_utils.dll](https://github.com/tauri-apps/nsis-tauri-utils/releases/download/nsis_tauri_utils-v0.4.1/nsis_tauri_utils.dll)和[NSIS-ApplicationID.zip](https://github.com/tauri-apps/binary-releases/releases/download/nsis-plugins-v0/NSIS-ApplicationID.zip)这两个文件

### 3.放置文件

1. 找到此路径`C:\Users\你的用户名\AppData\Local\tauri` 注意替换用户名，tauri文件夹如果不存在可以创建一个
2. 在`tauri`文件夹下创建`WixTools314`，将`wix314-binaries.zip`解压到这个文件夹下
3. 在`tauri`文件夹下创建`NSIS`，将`nsis-3.zip`解压到这个文件夹下
4. 将`NSIS-ApplicationID.zip`复制到 `...\NSIS\Plugins\x86-unicode`，然后将其解压到当前文件夹下
5. 将 `nsis_tauri_utils.dll`也复制到这个`x86-unicode`文件夹下

大功告成，再次构建就没问题了

![image-20250123122448099](https://s2.loli.net/2025/01/23/BjaG8x5WwzEdTpN.png)

这个过程还是挺麻烦的，希望官方后续有更好的解决办法，解决办法参考自[#7338](https://github.com/tauri-apps/tauri/issues/7338)
