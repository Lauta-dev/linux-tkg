# 🐸 linux-tkg — para una sola máquina

Kernel packages generados por CI directamente (`.github/workflows/arch-builder.yml`), listos para `pacman -U`.
Target único del build: la máquina de abajo — una Silvermont con 4 GiB de RAM y nada que probar más.

```text
██╗     ██╗███╗   ██╗██╗   ██╗██╗  ██╗    ████████╗██╗  ██╗ ██████╗
██║     ██║████╗  ██║██║   ██║╚██╗██╔╝    ╚══██╔══╝██║ ██╔╝██╔════╝
██║     ██║██╔██╗ ██║██║   ██║ ╚███╔╝        ██║   █████╔╝ ██║  ███╗
██║     ██║██║╚██╗██║██║   ██║ ██╔██╗        ██║   ██╔═██╗ ██║   ██║
███████╗██║██║ ╚████║╚██████╔╝██╔╝ ██╗       ██║   ██║  ██╗╚██████╔╝
╚══════╝╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝  ╚═╝       ╚═╝   ╚═╝  ╚═╝ ╚═════╝
                        [ para Silvermont · Arch Linux ]
```

## $ cat hardware.txt

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
> **Kernel personal.** Compilado para ese SoC, no para el tuyo.
> Quizás te sirva si tenés un Bay Trail/Braswell guardado, pero acá no se promete nada.

## $ cat for-whom.txt

- **builder:** @Lauta-dev, via GitHub Actions (`workflow_dispatch`)
- **user:** yo
- **local build:** `bore` + `silvermont` tune + gcc — el que corre todos los días
- **CI builds:** `eevdf` · `bore`, tune genérico `x86-64` + llvm — lo que se publica
- **artifacts:** `linux*tkg*.pkg.tar.zst` — kernel + headers, en cada release
  ([Releases](https://github.com/Lauta-dev/linux-tkg/releases))
- **tag:** `v<pkgver>-arch-<sched>`
- **upstream:** [Frogging-Family/linux-tkg](https://github.com/Frogging-Family/linux-tkg)

## $ cat build.why

- `_processor_opt="silvermont"` — es la µarch real del N2807; el tune existe en el compilador y es gratuito, así que se usa. Los builds de CI salen con `x86-64` baseline para poder caer en cualquier x86_64.
- `gcc` local / `llvm` en CI — dos toolchains, mismo kernel, el sabor no cambia el resultado de fondo.
- `_cpusched="bore"` — lo que de verdad se nota en un 2C sin AVX; `eevdf` queda de fallback seguro en CI.
- stripped + sin debug info — el `.pkg.tar.zst` final tiene que caber porque el disco es chico y la RAM tampoco da margen.

## $ gh release download

```bash
gh release download --repo Lauta-dev/linux-tkg --pattern 'linux*.pkg.tar.zst'
sudo pacman -U linux*-tkg-*.pkg.tar.zst
```

O directo desde la página de [Releases](https://github.com/Lauta-dev/linux-tkg/releases): los assets incluyen headers, para quien necesite DKMS.

## $ ls ../upstream-docs

```text
https://github.com/Frogging-Family/linux-tkg#readme
```

Ahí vive toda la docu real: `customization.cfg`, `install.sh`, procedimiento para Arch/Deb/RPM/Gentoo y compañía.
**Este README diverge a propósito** — y no tiene por qué seguir cambios de upstream.

<p align="right">:frog: <sub>compilado en dispatch, powered by gh-actions</sub></p>
