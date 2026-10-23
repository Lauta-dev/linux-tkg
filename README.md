```text
██╗     ██╗███╗   ██╗██╗   ██╗██╗  ██╗    ████████╗██╗  ██╗ ██████╗
██║     ██║████╗  ██║██║   ██║╚██╗██╔╝    ╚══██╔══╝██║ ██╔╝██╔════╝
██║     ██║██╔██╗ ██║██║   ██║ ╚███╔╝        ██║   █████╔╝ ██║  ███╗
██║     ██║██║╚██╗██║██║   ██║ ██╔██╗        ██║   ██╔═██╗ ██║   ██║
███████╗██║██║ ╚████║╚██████╔╝██╔╝ ██╗       ██║   ██║  ██╗╚██████╔╝
╚══════╝╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝  ╚═╝       ╚═╝   ╚═╝  ╚═╝ ╚═════╝
                        [ para Silvermont · Arch Linux ]
```

## Hardware

```text
OS:       EndeavourOS x86_64
Kernel:   6.12.105-273-tkg-bore
CPU:      Intel Celeron N2807 (2C/2T) @ 1.58→2.16 GHz · Silvermont · no AVX
GPU:      Intel Atom Z36xxx Series Graphics (integrada, Gen 7)
Memory:   4 GiB DDR3L (~3.7 usable) + zram 3.7 GiB
Disk (/): Kingston SA400S37 240 GB · SATA SSD
WiFi:     Realtek RTL8723BE
LAN:      Realtek RTL810xE
Audio:    Intel HDA (Atom Z36xxx)
```

> [!WARNING]
> **Kernel personal.** Compilado para ese SoC, no para el tuyo — acá no se promete nada.

## Opciones de build

- tune: `silvermont` en build local (nativo del N2807) · `x86-64` genérico en CI
- toolchain: `llvm`, acá y en CI — única que se usa
- scheduler: `bore` · `eevdf` (opción del dispatch de CI)
- stripped + sin debug info — el `.pkg.tar.zst` tiene que ser liviano

## Instalación

Descargá los assets de la última [release](https://github.com/Lauta-dev/linux-tkg/releases) (kernel + headers, tags tipo `v<pkgver>-arch-<sched>`) y:

```bash
sudo pacman -U linux*-tkg-*.pkg.tar.zst
```
