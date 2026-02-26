```
initramfs/
├── init          (executable shell script)
├── linuxrc       (symlink to init)
├── bin/          (BusyBox binary + symlinks: sh, ls, mkdir, mount, etc.)
│   └── busybox   (single static binary)
├── sbin/         (minimal: modprobe symlink)
├── lib/          (kernel modules only)
│   ├── modules/
│   │   └── 6.11.0/  (kernel version-specific)
│   │       ├── kernel/
│   │       │   ├── drivers/block/loop.ko.xz
│   │       │   ├── drivers/ata/ahci.ko.xz
│   │       │   ├── drivers/nvme/host/nvme.ko.xz
│   │       │   └── fs/ext4/ext4.ko.xz
│   │       └── modules.order  (module dep file)
│   └── modules.builtin.modinfo
└── proc/         (empty dir for mounting)
```
