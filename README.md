## Drivers WiFi TP-Link Archer T2U Plus AC 600

v4.2.2 working atm 30/10/2020

## Chipset Realtek RTL8811/12AU

## En caso de haber instalado drivers previamente, deberemos quitarlos con el siguiente comando:
```
sudo apt purge rtl8812au-dkms
```

## Comandos para instalación

```
$ sudo apt install git
$ sudo git clone https://github.com/cauid/archert2uplus.git
$ sudo cp -r archert2uplus  /usr/src/archert2uplus-4.2.2
$ sudo dkms add -m archert2uplus -v 4.2.2
$ sudo dkms build -m archert2uplus -v 4.2.2
$ sudo dkms install -m archert2uplus -v 4.2.2
```

Una vez instalado podemos reinciar el equipo. No detectará redes wifi sin hacerlo previamente.
Ayuda: en caso de estar instalado correctamente el USB tendrá una luz parpadenate.

## Source
https://ubuntuforums.org/showthread.php?t=2375603


## ---- Original text from original repository ----

## Changes
2019-07-11: Updated to compile against kernel 5.2

## Realtek 802.11ac (rtl8812au)

This is a fork of the Realtek 802.11ac (rtl8812au) v4.2.2 (7502.20130507)
driver altered to build on Linux kernel version >= 3.10.

### Purpose

My D-Link DWA-171 wireless dual-band USB adapter needs the Realtek 8812au
driver to work under Linux.

The current rtl8812au version (per nov. 20th 2013) doesn't compile on Linux
kernels >= 3.10 due to a change in the proc entry API, specifically the
deprecation of the `create_proc_entry()` and `create_proc_read_entry()`
functions in favor of the new `proc_create()` function.

### Building

The Makefile is preconfigured to handle most x86/PC versions.  If you are compiling for something other than an intel x86 architecture, you need to first select the platform, e.g. for the Raspberry Pi, you need to set the I386 to n and the ARM_RPI to y:
```sh
...
CONFIG_PLATFORM_I386_PC = n
...
CONFIG_PLATFORM_ARM_RPI = y
```

There are many other platforms supported and some other advanced options, e.g. PCI instead of USB, but most won't be needed.

The driver is built by running `make`, and can be tested by loading the
built module using `insmod`:

```sh
$ make
$ sudo insmod 8812au.ko
```

After loading the module, a wireless network interface named __Realtek 802.11n WLAN Adapter__ should be available.

### Installing

Installing the driver is simply a matter of copying the built module
into the correct location and updating module dependencies using `depmod`:

```sh
$ sudo cp 8812au.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless
$ sudo depmod
```

The driver module should now be loaded automatically.

### DKMS

Automatically rebuilds and installs on kernel updates. DKMS is in official sources of Ubuntu, for installation do:

```sh
$ sudo apt-get install build-essential dkms 
```

Install the driver to DKMS with:
```sh
sudo make dkms_install
```

Automatically load at boot:
```sh
$ echo 8812au | sudo tee -a /etc/modules
```

Eventually remove from DKMS with:
```sh
$ sudo make dkms_remove
```

### References

- D-Link DWA-171
  - [D-Link page](http://www.dlink.com/no/nb/home-solutions/connect/adapters/dwa-171-wireless-ac-dual-band-usb-adapter)
  - [wikidevi page](http://wikidevi.com/wiki/D-Link_DWA-171_rev_A1)
