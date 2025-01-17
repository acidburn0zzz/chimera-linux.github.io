---
layout: book
title: Booting
section: 2.3
---

Once you have prepared your media, you can boot from it. The boot
will vary depending on the image type you have used. Live images
use the GRUB bootloader. Device-specific images may use their own
bootloaders, but typically it is U-Boot.

## ISO images

### UEFI systems

This may vary with hardware, but in general a properly created
USB stick or CD/DVD disc should appear in the list of boot entries.

On the `x86_64` architecture, you will typically get a selection
between UEFI and BIOS mode, assuming CSM is not disabled. Pick
whichever you prefer, but keep in mind that this affects things
such as bootloader setup when installing.

### OpenPOWER systems

OpenPOWER systems use Petitboot. Simply boot your computer with
the removable media inserted and the respective boot entries should
appear.

### Qemu virtual machines

When using virtual machines, you can pass the image like this:

```
-cdrom /path/to/chimera.iso -boot d
```

### Serial console

If you wish to use a serial terminal, you might have to do some
additional setup, depending on the configuration.

In a lot of cases, the kernel will output to serial console
automatically, without doing anything. This is especially the
case if you don't have a graphical output. However, if you do
not get kernel output on your serial terminal (i.e. if the
bootloader does appear but the kernel messages do not) you
will have to enable it manually, with the `console=` parameter.

On most `x86_64` setups, this will be `console=ttyS0`.

On most POWER setups, `console=hvc0` is what you want. On some
other POWER systems this might be `console=hvsi0`.

AArch64 and RISC-V systems vary. Refer to the documentation for your
system. Examples include `ttyAMA0`, `ttyS2`, `ttymxc0`, `ttySIF0`
and others.

The Chimera live images are set up to automatically enable a
login prompt (`getty`) for all consoles the kernel outputs to.

### Picking the boot option

Console images come with two boot options, regular boot and RAM
boot. The latter results in the whole system being copied to system
RAM, while the former will create a writable overlay over a read-only
mount.

The RAM option requires a large amount of memory. Unless you are sure,
you should be using the regular option. The benefit of the RAM option
is that the system will run faster, and especially for optical media,
will not result in accesses to the media.

Desktop images come with additional boot options to force console
boot (the default is to boot into GNOME desktop with Wayland). It
is also possible to force X11 by editing the graphical boot option
and adding the `nowayland` kernel command line parameter, but keep
in mind that GNOME currently has issues under X11 with most accelerated
drivers (software rendering works fine, so you may use it on systems
with unaccelerated 2D framebuffers) which may result in it freezing
on the first frame. Therefore, it is highly recommended to always
use Wayland for GNOME (X11 works for other window managers/desktops).

### Logging in

Once this is set up properly, you will be presented with a login
prompt on console images. Graphical boots bring you directly to
desktop without having to log in.

You will want to use `anon` or `root` as the user name (depending
on if you want a superuser) with the password `chimera`. If you
log in with `anon`, use the `doas` utility to gain superuser
privileges.

## Device images

Device images are pre-made so that they boot out of box on whichever
device they made for.

There is no regular user. Log in with `root`, password `chimera`. If
your device supports serial console, it should be set up and working
by default, so there is nothing to configure.

Device images never come with a graphical desktop environment, but
you can install one if you need one.

If the media you have flashed the image to is your final boot media
and you will not be installing anywhere else, you can skip directly
to [Configuration](/docs/configuration) as there is nothing else to
do.
