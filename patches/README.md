# patches/

## 0100-ossi-projectinfo.patch — TIDAK DIPAKAI (REVERTED)

Patch ini menambahkan EXPORT_SYMBOL get_project/get_Modem_Version/
get_Operator_Version ke kernel. Ternyata BERBAHAYA: modul vendor
`oplus_bsp_boot_projectinfo.ko` sendiri mengekspor ketiganya, dan kernel
yang juga mengekspornya membuat modul itu DITOLAK loader
("exports duplicate symbol get_Modem_Version (owned by kernel)").

Penyedia yang benar = projectinfo.ko (persis seperti kernel Kuchiba +
Android bekerja). Diagnosa: kallsyms sistem kerja menunjukkan
"T get_project [oplus_bsp_boot_projectinfo]".

Jangan apply patch ini kecuali ada alasan kuat di masa depan.
