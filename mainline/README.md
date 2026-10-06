# Kernel mainline untuk OPPO A38 (ossi)

Basis: [mt6768-mainline/linux](https://github.com/mt6768-mainline/linux)
(kernel 6.16, komunitas — SoC sama: MT6768/6769 = Helio G80/G85/P65),
di-pin commit `6fceef1` (branch `cleanup`).

- `mt6768-oppo-ossi.dts` — DT A38, diadaptasi dari `mt6768-xiaomi-merlin.dts`
  (Redmi Note 9, Helio G85). v1 bring-up: eMMC (msdc0), microSD, USB (MUSB+OTG),
  PMIC MT6358, UART0. Belum: panel DSI ili7807s, touch ilitek7807s, WiFi/BT,
  kamera, charger oplus.
- `config-ossi-mainline.aarch64` — config dari pmaports upstream
  (linux-postmarketos-mediatek-mt6768) + LOCALVERSION=-ossi-mainline.
- `tools/mkbootimg.py` — untuk membungkus boot.img header v2 + DTB
  (pola yang sama dipakai lancelot/merlin; perlu diuji apakah lk A38 menerimanya).

Hasil build di workflow `Build Mainline Kernel (mt6768) OPPO A38`:
`boot-mainline.img` — flash ke `boot_b`, dipasangkan dengan initramfs pmOS
yang sudah ada di `init_boot_b` (urutan load modul vendor akan dilewati
secara alami karena semua driver sudah built-in di kernel mainline).
