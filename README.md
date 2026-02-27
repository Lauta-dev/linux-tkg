# 🚀 XanMod 5.15 Slim - Silvermont Edition

Este repositorio contiene un flujo de trabajo automatizado para compilar un kernel **XanMod 5.15** ultra-ligero, optimizado específicamente para la microarquitectura **Intel Silvermont**.

## 💻 Target Hardware Specs
El kernel se compila siguiendo este perfil de hardware extraído del host:

```text
Arquitectura:                        x86_64
modo(s) de operación de las CPUs:    32-bit, 64-bit
Tamaños de las direcciones:          36 bits physical, 48 bits virtual
Orden de los bytes:                  Little Endian
CPU(s):                              2
Lista de la(s) CPU(s) en línea:      0,1
ID de fabricante:                    GenuineIntel
Nombre del modelo:                   Intel(R) Celeron(R) CPU N2807 @ 1.58GHz
Familia de CPU:                      6
Modelo:                              55
Hilo(s) de procesamiento por núcleo: 1
Núcleo(s) por «socket»:              2
«Socket(s)»:                         1
Revisión:                            8
CPU(s) factor de escala MHz:         83%
CPU MHz máx.:                        2165,8000
CPU MHz mín.:                        499,8000
BogoMIPS:                            3166,66
Virtualización:                      VT-x
Caché L1d:                           48 KiB (2 instancias)
Caché L1i:                           64 KiB (2 instancias)
Caché L2:                            1 MiB (1 instancia)
```

## 🛠️ Optimizaciones Aplicadas

Para exprimir el N2807 en juegos como NFS Most Wanted, el kernel incluye:
- Micro-Architecture: CONFIG_MATOM (Silvermont/Airmont).
- Preemption: PREEMPT_FULL (Respuesta inmediata, menos micro-stuttering).
- Compression: LZ4 (Carga el kernel en RAM casi instantáneamente).
- Modprobed-db: Eliminación masiva de drivers innecesarios (Kernel Diet).
- Tuning: Mitigaciones de CPU ajustadas para no castigar el rendimiento del Celeron.

## 📦 Instalación
1. Descarga el .tar.gz desde [Releases](https://github.com/Lauta-dev/linux-tkg/releases) .

2. Extrae en la raíz:
```Bash
sudo tar -xzvf kernel-xanmod-515-slim.tar.gz -C /
```

3. Genera el Initramfs con Dracut:
```Bash
sudo dracut --force --hostonly --compress lz4 --kver 5.15.95-xanmod1 /efi/initramfs-515-xanmod-silvermont.img
```

4. Añade una nueva entrada
```bash
sudo nano /efi/loader/entries/xanmod-slim.conf
```

5. Pegar este contenido
```file
title   XanMod 5.15 Slim (Silvermont)
linux   /vmlinuz-515-xanmod-silvermont
initrd  /initramfs-515-xanmod-silvermont.img
options root=UUID=xxx rw intel_pstate=passive quiet loglevel=3 mitigations=off
```

>[!info]
> el UUID=xxx se consigue con:
>```bash
>lsblk -no UUID $(df / | tail -n1 | cut -d' ' -f1)
>```
