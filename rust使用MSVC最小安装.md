---
title: rust使用MSVC最小安装
date: 2025-02-16 15:55:32
categories: tools
tags:
  - rust
  - msvc
cover: https://s2.loli.net/2024/06/09/oDMZ8ErVu49t5lv.webp
---

## 1.安装VS生成工具

前往[VS下载界面](https://visualstudio.microsoft.com/zh-hans/downloads/)，下滑到底部，找到**VS 2022 生成工具**并下载，下载后双击`vs_BuildTools.exe`文件安装

![image-20250216160148429](https://s2.loli.net/2025/02/16/CTsn8dPXmDUNQch.png)

安装完成后会打开这个界面

![image-20250216160507503](https://s2.loli.net/2025/02/16/Qe8awAkNZCGlsqJ.png)

## 2.安装环境

这个安装过程参考[官方文档](https://rust-lang.github.io/rustup/installation/windows-msvc.html)

> 为什么选msvc？c/c++环境也可以使用mingw，而且体积相对来说小很多，不过我之前rust使用mingw作为工具链时会出现各种各样的问题，实在是不想折腾了（主要出现的问题也不好解决），还是使用msvc**更省事**

### 2.1 MSVC

点击单个组件，搜索`MSVC v143 - VS 2022 C++ x64/x86 build tools`，勾选上

![image-20250216161213836](https://s2.loli.net/2025/02/16/ySs8HzVZMLKe9XN.png)

### 2.2 Windows SDK

搜索`windows`，找到对应win版本的SDK下载，我使用的win11，所以选择win11 SDK，直接勾选最新版本就行

![1739693610135_d](https://s2.loli.net/2025/02/16/jSA5V1b7DNFMoqk.png)

### 2.3 语言包

点击语言包，勾选英语

![1739693979799_d](https://s2.loli.net/2025/02/16/DAfg82eYnQxJZlM.png)

### 2.4 安装

点击顶部安装位置可以修改一个合适的安装位置，查看右边单个组件无误后点击安装即可

![image-20250216161810237](https://s2.loli.net/2025/02/16/r1ySNmMAk67hxJ4.png)

### 2.5 添加环境

> 安装后的MSVC并不能识别，必须添加环境变量，在MSVC安装文件夹下找到`cl.exe`文件，路径大概是`A:\env\MSVC\VC\Tools\MSVC\14.43.34808\bin\Hostx64\x64`，注意前面的`A:\env\MSVC`是我的安装路径

<kbd>win + I</kbd>打开电脑设置，然后搜索环境，选择**编辑系统环境变量**，打开**环境变量**，在path变量列表添加该路径即可

![image-20250216164719324](https://s2.loli.net/2025/02/16/mb7xLVZctN9RYIl.png)



![image-20250216164045675](https://s2.loli.net/2025/02/16/hVm8P2LIz7bxuJe.png)

打开控制台输入`cl`验证

![image-20250216164127953](https://s2.loli.net/2025/02/16/EBK8OIvrxcZGD4t.png)

## 3.安装rust

### 3.1 下载rust-init

在[rust官网](https://www.rust-lang.org/zh-CN/tools/install)下载`rustup-init.exe`

### 3.2 自定义安装路径（可选）

安装前可以设置rustup和cargo的安装路径，还是打开环境变量，新增2个变量

- `RUSTUP_HOME` 值设置为rustup安装路径
- `CARGO_HOME` 值设置为cargo安装路径

### 3.3 设置镜像源（可选但是推荐，国内下载很慢）

以[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/help/rustup/)为例

还是新增环境变量

- 变量名：`RUSTUP_UPDATE_ROOT` 值：`https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup`
- 变量名：`RUSTUP_DIST_SERVER` 值：`https://mirrors.tuna.tsinghua.edu.cn/rustup`

### 3.4 安装rust

双击`rustup-init.exe`文件,会出现下面一些选项，直接回车也就是选第一个

```powershell
Current installation options:


   default host triple: x86_64-pc-windows-msvc
     default toolchain: stable (default)
               profile: default
  modify PATH variable: yes

1) Proceed with standard installation (default - just press enter)
2) Customize installation
3) Cancel installation
>
```

出现这个界面就算是成功了

![image-20250216170410261](https://s2.loli.net/2025/02/16/NWSqVszD8cxOFHi.png)


### 3.5 验证

重新打开一个终端窗口，输入下面命令，能正常输出版本则安装成功了

```powershell
rustc --version
cargo --version
```

新建一个 `main.rs`文件，写入以下内容

```rust
fn main() {
    println!("Hello, Rust!");
}
```

在终端执行命令

```powershell
rustc main.rs
./main
```

输出结果应为`Hello, Rust!`
