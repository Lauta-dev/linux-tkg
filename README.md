# Linux TKG para Silvermont
Linux TKG compilado para gaming y uso diario, optimizado para arquitectura Intel Silvermont.

---

## ⚠️ Compatibilidad

**Solo soporta paquetes para Arch Linux.**
No está testeado ni garantizado en otras distribuciones.

---

## 🖥️ Hardware Target

| Propiedad | Valor |
|-----------|-------|
| CPU | Intel Celeron N2807 @ 1.58GHz |
| Arquitectura | x86_64 (Silvermont) |
| Núcleos | 2 (1 hilo por núcleo) |
| Caché L1d | 48 KiB |
| Caché L1i | 64 KiB |
| Caché L2 | 1 MiB |
| Frecuencia máx. | 2165 MHz |
| Virtualización | VT-x |

---

## ⚙️ Configuración del Kernel

| Opción | Valor |
|--------|-------|
| `_cpusched` | `pds` |
| `_processor_opt` | `silvermont` |
| Patchset | TKG default |
| Módulos | `modprobed.db` (solo módulos usados) |

### ¿Qué es PDS?
PDS (Priority and Deadline based Scheduler) es un scheduler alternativo orientado a baja latencia e interactividad, ideal para gaming y uso de escritorio.

### ¿Qué es modprobed.db?
En lugar de compilar todos los módulos del kernel, se usa una base de datos (`modprobed.db`) con solo los módulos que el sistema realmente utiliza. Esto reduce el tiempo de compilación y el tamaño del kernel resultante.

---

## 🗂️ Estructura del repositorio
```
linux-tkg-silvermont/
├── customization.cfg          # Original de linux-tkg, NO modificar
├── silvermont.cfg             # ← Tu fuente de verdad, config para N2807
├── modprobed.db               # Módulos usados por el hardware target
└── .github/
    └── workflows/
        └── build.yml          # GitHub Action para compilar automáticamente
```

> **Importante:** `customization.cfg` es el archivo original de linux-tkg y **no debe modificarse directamente**. La configuración real está en `silvermont.cfg`. El workflow lo copia automáticamente antes de compilar:
> ```bash
> cp silvermont.cfg customization.cfg
> ```
> Esto evita conflictos al sincronizar con el upstream de linux-tkg.

---

## 📦 Instalación

### Requisitos previos
```bash
sudo pacman -S base-devel git
yay -S modprobed-db
```

### Generar tu modprobed.db

> **Importante:** El `modprobed.db` incluido en este repo fue generado para el hardware listado arriba. Si tu hardware es diferente, generá el tuyo:
```bash
sudo modprobed-db storesilent
modprobed-db list
```

### Clonar y compilar
```bash
git clone https://github.com/TU_USUARIO/linux-tkg-silvermont
cd linux-tkg-silvermont
makepkg -si
```

---

## 🌿 Ramas

| Rama | Descripción |
|------|-------------|
| `master` | Sincronizada con linux-tkg upstream |
| `silvermont` | Config con modprobed.db para N2807 |

---

## 🏷️ Tags

Los tags marcan versiones estables:
```
6.12-silvermont  →  Kernel 6.12 estable para Silvermont
```

---

## 🙏 Créditos

Basado en [linux-tkg](https://github.com/Frogging-Family/linux-tkg) de **Frogging-Family**.

---

## ⚠️ Vulnerabilidades conocidas del hardware

El N2807 tiene vulnerabilidades conocidas (Meltdown, Spectre v1/v2, MDS) sin mitigación completa disponible. Es una limitación del hardware.
```bash
grep . /sys/devices/system/cpu/vulnerabilities/*
```
