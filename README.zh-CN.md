# rtl8733bs

[English](README.md) | 简体中文

NanoKVM PCIe 选配 Wi-Fi 模块使用的 Realtek RTL8733BS SDIO Wi-Fi 和蓝牙驱动。

| 目录 | 模块 |
|---|---|
| `rtl8733bs` | `8733bs.ko`（Wi-Fi，Realtek 驱动 v5.14.1-46） |
| `rtl8733bs_bt_sdio` | `btrtksdio.ko`（蓝牙；使用 `8733bs.ko` 导出的符号） |

源码来自 LicheeRV Nano SDK 的 `osdrv/extdrv/wireless`。提交历史只保留这两个目录，并包含适配 Linux 6.18 的改动。

## 构建

先构建 Wi-Fi 模块，再让蓝牙模块使用它生成的 `Module.symvers`：

```sh
make -C "$KERNEL_BUILD" M="$PWD/rtl8733bs" \
    ARCH=riscv CROSS_COMPILE=riscv64-linux- CONFIG_BR_EXT=n modules
make -C "$KERNEL_BUILD" M="$PWD/rtl8733bs_bt_sdio" \
    ARCH=riscv CROSS_COMPILE=riscv64-linux- \
    KBUILD_EXTRA_SYMBOLS="$PWD/rtl8733bs/Module.symvers" modules
```

OneKVM 用 `onekvm-distro` 中的 `onekvm-device-nanokvm-wireless-modules` 配方构建并打包这两个模块。

## 许可证

GPL-2.0-only。见 `LICENSE` 和各源码文件头。
