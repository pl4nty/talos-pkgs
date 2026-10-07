# Kernel

## Updating kernel config

When updating kernel to the new version, import proper defaults with:

```sh
make kernel-olddefconfig USERNAME=rsmitty
```

If you want to update for a specific architecture only, use:

```sh
make kernel-olddefconfig USERNAME=rsmitty PLATFORM=linux/arm64
```

## Customizing the kernel

Run another target to get into `menuconfig`:

```sh
make kernel-menuconfig USERNAME=rsmitty
```

## Testing

- Build and push a test image with `make USERNAME=rsmitty PUSH=true kernel`
- PR upstream (when ready) and profit

## Built-in drivers

Talos prefers modules, but a few drivers must be built in (`=y`).
Note each one here, because `kernel-olddefconfig` drops comments from the config files.

| Option | Arch | Why it is built in |
| --- | --- | --- |
| `CONFIG_PINCTRL_TH1520` | riscv64 | Most T-Head TH1520 devices (Lichee Pi 4A and others) take their pins from this controller. That includes the `ttyS0` console, the GPIO controllers and everything that uses a GPIO. As a module it only loads when Talos loads modules, at about 32 s on the Lichee Pi 4A. Until then the console is silent, `gpio-dwapb` logs about 20 `failed to register gpiochip` deferral errors, and the GPIO expander, Wi-Fi power sequencing and both GMACs start about 30 s late. Tested on a Lichee Pi 4A on 2026-10-07. |
