```
██╗     ██╗███╗   ██╗██╗   ██╗██╗  ██╗    ████████╗██╗  ██╗ ██████╗
██║     ██║████╗  ██║██║   ██║╚██╗██╔╝    ╚══██╔══╝██║ ██╔╝██╔════╝
██║     ██║██╔██╗ ██║██║   ██║ ╚███╔╝        ██║   █████╔╝ ██║  ███╗
██║     ██║██║╚██╗██║██║   ██║ ██╔██╗        ██║   ██╔═██╗ ██║   ██║
███████╗██║██║ ╚████║╚██████╔╝██╔╝ ██╗       ██║   ██║  ██╗╚██████╔╝
╚══════╝╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝  ╚═╝       ╚═╝   ╚═╝  ╚═╝ ╚═════╝
                        [ para Silvermont · Arch Linux ]
```

> **linux-tkg compilado para gaming y uso diario.**  
> Optimizado para Intel Celeron N2807 (Bay Trail / Silvermont) con scheduler PDS y modprobed-db.

---

## ⚠️ ADVERTENCIA

**Solo soporta paquetes para Arch Linux.**  
No probado ni garantizado en otras distribuciones. Usalo bajo tu propio riesgo.

---

## 🖥️ Hardware Target
```
CPU     : Intel(R) Celeron(R) CPU N2807 @ 1.58GHz
Arch    : x86_64 (Silvermont / Bay Trail)
Cores   : 2 físicos · 1 hilo por núcleo
L1d     : 48 KiB  |  L1i : 64 KiB  |  L2 : 1 MiB
Freq    : 499 MHz (min) → 1580 MHz (base) → 2165 MHz (boost)
Virt    : VT-x habilitado
```

---

## ⚙️ Config del Kernel
```
_cpusched       = pds
_processor_opt  = silvermont
_compiler       = gcc
_lto_mode       = none
_modprobeddb    = true
_debugdisable   = true
_ftracedisable  = true
_timer_freq     = 1000
_default_cpu_gov= performance
_tickless       = 2
_custom_cmdline = intel_pstate=passive split_lock_detect=off mitigations=off nowatchdog
patchset        = linux-tkg default (zenify + glitched_base + clear_patches)
```

### ¿Por qué PDS?
PDS (Priority and Deadline based Scheduler) es un scheduler alternativo de baja latencia, diseñado para interactividad y gaming. A diferencia de CFS/EEVDF, prioriza respuesta en lugar de throughput puro — ideal para hardware con pocos núcleos como el N2807.

### ¿Por qué modprobed-db?
En lugar de compilar los ~6000 módulos del kernel stock, `modprobed-db` genera una lista con **solo los módulos que tu sistema realmente usa**. En hardware Silvermont esto reduce el tiempo de compilación de ~2h a ~20min y produce un kernel más liviano.

### ¿Por qué `mitigations=off`?
El N2807 tiene vulnerabilidades conocidas (Meltdown, Spectre v1/v2, MDS) cuyas mitigaciones tienen un costo de rendimiento significativo en CPUs de esta generación. En un sistema de uso personal sin datos sensibles, deshabilitarlas recupera rendimiento real.

> **Si no estás de acuerdo con esto, quitá `mitigations=off` del cmdline antes de compilar.**

---

## 🗂️ Estructura del Repo
```
linux-tkg-silvermont/
├── customization.cfg          ← Original de linux-tkg. NO modificar.
├── silvermont.cfg             ← Tu fuente de verdad. Referencia de config.
├── modprobed.db               ← Módulos del hardware target (N2807)
├── *.myfrag                   ← Config fragments aplicados al build
└── .github/
    └── workflows/
        └── build.yml          ← CI: compila y publica releases automáticamente
```

> `customization.cfg` **no se modifica directamente**. Las variables de config se pasan como `env:` en el workflow, sobreescribiendo el cfg en tiempo de build. Esto evita conflictos al sincronizar con el upstream de linux-tkg.

---

## 📦 Instalación

### Desde GitHub Releases (recomendado)

Bajá los `.pkg.tar.zst` de la sección [Releases](../../releases) e instalá con pacman:
```bash
sudo pacman -U linux-tkg-pds-*.pkg.tar.zst linux-tkg-pds-headers-*.pkg.tar.zst
```

### Compilar desde fuente

#### Requisitos
```bash
sudo pacman -S base-devel git
yay -S modprobed-db
```

#### Generar tu propio modprobed.db

> El `modprobed.db` incluido fue generado en el hardware target (N2807). Si tu hardware es diferente, generá el tuyo:
```bash
sudo modprobed-db storesilent
modprobed-db list
```

Cuanto más tiempo uses tu sistema antes de compilar, más completa va a ser la db.

#### Build
```bash
git clone https://github.com/TU_USUARIO/linux-tkg-silvermont
cd linux-tkg-silvermont
makepkg -si
```

---

## 🤖 CI / GitHub Actions

El workflow `.github/workflows/build.yml` se ejecuta automáticamente en cada push a `silvermont` y genera un Release con los paquetes listos para instalar.
```
push → build en Arch container → makepkg → upload artifacts → GitHub Release
```

La config del kernel vive en las variables `env:` del workflow — no en el `customization.cfg` — para sobrevivir syncs con upstream.

---

## 🌿 Ramas

| Rama | Descripción |
|------|-------------|
| `master` | Sincronizada con linux-tkg upstream |
| `silvermont` | Config + CI para Intel N2807 con modprobed-db |

---

## 🏷️ Tags
```
v6.12.x-pds  →  Kernel 6.12.x · scheduler PDS · Silvermont
```

---

## ⚠️ Vulnerabilidades del hardware
```bash
$ grep . /sys/devices/system/cpu/vulnerabilities/*

mds              : Vulnerable; SMT vulnerable
meltdown         : Vulnerable
spectre_v1       : Vulnerable: __user pointer sanitization and usercopy barriers only
spectre_v2       : Vulnerable; IBPB: disabled; STIBP: disabled
mmio_stale_data  : Unknown: No mitigations
```

Limitaciones del hardware (Silvermont, 2013). No hay mitigación completa disponible independientemente del kernel que uses.

---

## 🙏 Créditos

Basado en **[linux-tkg](https://github.com/Frogging-Family/linux-tkg)** de [Frogging-Family](https://github.com/Frogging-Family).  
Todo el trabajo del patchset, sistema de compilación y schedulers alternativos es de ellos.

---

<div align="center">
<sub>built with ❤️ for old hardware that still kicks</sub>
</div>
