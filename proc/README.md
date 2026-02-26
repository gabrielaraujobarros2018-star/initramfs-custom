# /proc - Everything via Your Custom /bin/busybox

If /bin/busybox already exists, create symlinks:
```
mkdir -p /sbin
ln -s /bin/busybox /sbin/init
ln -s /bin/busybox /sbin/modprobe
ln -s /bin/busybox /sbin/reboot
ln -s /bin/busybox /sbin/poweroff
ln -s /bin/busybox /sbin/switch_root
```

Then extend your /bin/busybox script:
```
case "$cmd" in
    init)
        exec /init
        ;;
    reboot)
        echo b > /proc/sysrq-trigger
        ;;
    poweroff)
        echo o > /proc/sysrq-trigger
        ;;
    modprobe)
        echo "Module loading not implemented"
        ;;
```

# /proc - True Minimal Static Tools

If you're building a more real system, /sbin typically contains binaries from:
- busybox
- or full utilities from GNU coreutils + kmod

example real layout:
```
/sbin/init
/sbin/modprobe
/sbin/depmod
/sbin/switch_root
```

# Absolute Minimal Working Setup

For a kernel that boots directly into /init, you don't even need /sbin/init.

Minimal tree:
```
/
 ├── init
 ├── bin/
 ├── sbin/
 ├── proc/
 ├── sys/
 └── dev/
```

Where /init contains:
```
#!/bin/sh
mount -t proc proc /proc
mount -t sysfs sysfs /sys
mount -t devtmpfs devtmpfs /dev
exec /bin/sh
```

In that case, /sbin can be empty.

# When /sbin/init Is Required

If your kernel command line contains:
```
init=/sbin/init
```

or no init= is specified and /init doesn't exist, the kernel searches in this order:
- /init
- /sbin/init
- /etc/init
- /bin/init
- /bin/sh

So /sbin/init becomes important in traditional layouts.
