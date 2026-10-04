# Kernel-mt6769-a38-PMOS

Kernel **postmarketOS/Nura** untuk **OPPO A38 (CPH2579, codename `ossi`)** — MediaTek Helio G85 (MT6769V/CZ, platform `mt6768`/`k69v1_64`), Linux **6.6.30**.

Repo ini **tidak berisi source kernel**. Source ada di [giel-arch/kernel-mt6769-a38](https://github.com/giel-arch/kernel-mt6769-a38) (Android GKI 6.6, `oppo_a38_defconfig`). Repo ini berisi:

1. **`pmos.fragment`** — fragmen Kconfig yang di-merge di atas `oppo_a38_defconfig`.
2. **Workflow GitHub Actions** yang: clone source kernel → merge fragmen → build `Image.gz` (clang r510928, sama seperti kernel stok/aktif) → repack `assets/boot_b.bin` (boot stok, kernel saja) menjadi `boot-pmos.img`.
3. **`assets/boot_b.bin`** — boot image header v4 stok dari slot B (sebagai template repack).

## Kenapa fragmen ini penting

Percobaan port pmOS sebelumnya gagal karena kernel tidak punya `CONFIG_DEVTMPFS=y`:
initramfs pmOS melakukan `mount -t devtmpfs dev /dev`, jadi tanpa devtmpfs direktori
`/dev` kosong dan init tidak pernah menemukan partisi. Lihat komentar di `pmos.fragment`.

## Strategi driver

Kernel GKI ini **tidak** memuat driver platform MTK (mtk-mmc, clk-mt6768, pmic-wrap,
musb, dll). Modul-modul vendor itu ada di **vendor_boot** (vendor_ramdisk) dan di
**rootfs** (paket `mtk-vendor-modules` di repo
[PostmarketOS-For-A38](https://github.com/giel-arch/PostmarketOS-For-A38)). Karena
vermagic modul vendor ≠ vermagic build kita, modul dimuat paksa dengan
`force_insmod` (`finit_module` + `MODULE_INIT_IGNORE_VERMAGIC|MODVERSIONS`).
`CONFIG_MODULE_FORCE_LOAD=y` dan signature dimatikan di fragmen untuk itu.

## Output workflow

- `Image.gz` — kernel ter-compress
- `boot-pmos.img` — siap flash ke `boot_b`: `fastboot flash boot_b boot-pmos.img`
- `.config` — konfigurasi final (untuk debugging)

Repo pasangan (image OS lengkap): [PostmarketOS-For-A38](https://github.com/giel-arch/PostmarketOS-For-A38).
