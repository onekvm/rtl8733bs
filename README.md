# rtl8733bs

English | [简体中文](README.zh-CN.md)

Realtek RTL8733BS SDIO Wi-Fi and Bluetooth drivers for the optional Wi-Fi
module on NanoKVM PCIe.

| Directory | Module |
|---|---|
| `rtl8733bs` | `8733bs.ko` (Wi-Fi, Realtek driver v5.14.1-46) |
| `rtl8733bs_bt_sdio` | `btrtksdio.ko` (Bluetooth; uses symbols from `8733bs.ko`) |

The sources come from `osdrv/extdrv/wireless` in the LicheeRV Nano SDK. The
commit history keeps only these two directories and carries changes for
Linux 6.18.

## Build

Build the Wi-Fi module first, then point the Bluetooth module at its
`Module.symvers`:

```sh
make -C "$KERNEL_BUILD" M="$PWD/rtl8733bs" \
    ARCH=riscv CROSS_COMPILE=riscv64-linux- CONFIG_BR_EXT=n modules
make -C "$KERNEL_BUILD" M="$PWD/rtl8733bs_bt_sdio" \
    ARCH=riscv CROSS_COMPILE=riscv64-linux- \
    KBUILD_EXTRA_SYMBOLS="$PWD/rtl8733bs/Module.symvers" modules
```

OneKVM builds and packages the modules with the
`onekvm-device-nanokvm-wireless-modules` recipe in `onekvm-distro`.

## License

GPL-2.0-only. See `LICENSE` and the headers of the source files.
