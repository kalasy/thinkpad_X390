
# Config Debian testing Lenovo Thinkpad X390.

If you want to run Debian on your ThinkPad X390, you have come to the right place. 

### Disclaimer

This guide is intended for educational purposes only. Use at your own risk. 


## Table of contents
1. [Hardware](#Hardware)
2. [What is working](#Whatisworking)
3. [SmartCard](#SmartCard)
    - [DNIe](#dnie)
    - [autofirma](#autofirma)
4. [Fingerprint](#Fingerprint)
5. [Sound](#Sound)
6. [Resources](#Resources)

## Hardware <a name="Hardware"></a>

| Specifications | Details |
|:---|:---|
| Computer Model | ThinkPad X390 |
| CPU | Intel(R) Core(TM) i5-8265U CPU @ 1.60GHz |
| Model |  Lenevo 20Q0000KSP |
| Displayer | Lenovo LEN4094 ( 13.3 inch  ) |
| Memory | 8GB soldered memory, not upgradable! ( DDR4 2400 MHz ) |
| NVMe SSD | 256GB SSD M.2 2280 PCIe 3.0x4 NVMe Opal2 |
| Integrated Graphics | Intel UHD Graphics 630 (Mobile) |
| Ethernet |  Intel(R) Ethernet Connection (6) I219-V |
| Sound Card | Intel Intel Smart Sound Technology Audio Controller (layout-id: 11) |
| Wireless Card |  Intel(R) Wireless-AC 9560 160MHz |
| I/O |  2xUSB3.1, 1xUSB-C Thunderbolt 3, Smart card reader, HDMI 1.4b, Headphone/mic combo |

## What is working <a name="Whatisworking"></a>

Works out of the box mostly everithing but some hardware needs to be configured properly.

```
lsusb 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 058f:9540 Alcor Micro Corp. AU9540 Smartcard Reader
Bus 001 Device 003: ID 04ca:7070 Lite-On Technology Corp. Integrated Camera
Bus 001 Device 004: ID 06cb:00bd Synaptics, Inc. Prometheus MIS Touch Fingerprint Reader
Bus 001 Device 006: ID 0bda:5411 Realtek Semiconductor Corp. RTS5411 Hub
Bus 001 Device 008: ID 6820:0048 Generic HD camera 
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```

```
lspci
00:00.0 Host bridge: Intel Corporation Coffee Lake HOST and DRAM Controller (rev 0c)
00:02.0 VGA compatible controller: Intel Corporation WhiskeyLake-U GT2 [UHD Graphics 620] (rev 02)
00:04.0 Signal processing controller: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem (rev 0c)
00:08.0 System peripheral: Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model
00:12.0 Signal processing controller: Intel Corporation Cannon Point-LP Thermal Controller (rev 30)
00:14.0 USB controller: Intel Corporation Cannon Point-LP USB 3.1 xHCI Controller (rev 30)
00:14.2 RAM memory: Intel Corporation Cannon Point-LP Shared SRAM (rev 30)
00:14.3 Network controller: Intel Corporation Cannon Point-LP CNVi [Wireless-AC] (rev 30)
00:16.0 Communication controller: Intel Corporation Cannon Point-LP MEI Controller #1 (rev 30)
00:1c.0 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #1 (rev f0)
00:1c.4 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #5 (rev f0)
00:1d.0 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #9 (rev f0)
00:1d.4 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #13 (rev f0)
00:1f.0 ISA bridge: Intel Corporation Cannon Point-LP LPC Controller (rev 30)
00:1f.3 Audio device: Intel Corporation Cannon Point-LP High Definition Audio Controller (rev 30)
00:1f.4 SMBus: Intel Corporation Cannon Point-LP SMBus Controller (rev 30)
00:1f.5 Serial bus controller: Intel Corporation Cannon Point-LP SPI Controller (rev 30)
01:00.0 SD Host controller: Genesys Logic, Inc GL9750 SD Host Controller (rev 01)
02:00.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] (rev 01)
03:00.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] (rev 01)
03:01.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] (rev 01)
03:02.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] (rev 01)
04:00.0 System peripheral: Intel Corporation JHL6240 Thunderbolt 3 NHI (Low Power) [Alpine Ridge LP 2016] (rev 01)
3a:00.0 USB controller: Intel Corporation JHL6240 Thunderbolt 3 USB 3.1 Controller (Low Power) [Alpine Ridge LP 2016] (rev 01)
3d:00.0 Non-Volatile memory controller: Sandisk Corp SanDisk Extreme Pro / WD Black SN750 / PC SN730 / Red SN700 NVMe SSD
```
## SmartCard <a name="SmartCard"></a>

openjdk-21-jdk libnss3-tools are only needed if you plan to use autofirma (only for Spanish citizens) because this app needs java installed.
```
sudo apt -y install pcscd pcsc-tools libccid pinentry-gtk2  openjdk-21-jdk libnss3-tools
```

Then add to Firefox this module in order to use the smart card reader. Just locate the path for the file named "opensc-pkcs11.so" https://packages.debian.org/trixie/amd64/opensc-pkcs11/filelist, normally will be:

/usr/lib/x86_64-linux-gnu/opensc-pkcs11.so

[Old example with pictures](https://github.com/OpenSC/OpenSC/wiki/Installing-OpenSC-PKCS11-Module-in-Firefox,-Step-by-Step)


### DNIe <a name="DNIe"></a>

Now you can test the smartcard reader with the command "pcsc_scan"

```
pcsc_scan
PC/SC device scanner
V 1.7.3 (c) 2001-2024, Ludovic Rousseau <ludovic.rousseau@free.fr>
Using reader plug'n play mechanism
Scanning present readers...
0: Alcor Micro AU9540 00 00
 
Mon Aug  4 17:15:41 2025
 Reader 0: Alcor Micro AU9540 00 00
  Event number: 0
  Card state: Card removed, 
   
Mon Aug  4 17:15:43 2025
 Reader 0: Alcor Micro AU9540 00 00
  Event number: 1
  Card state: Card inserted, 
  ATR: 4B 7F 96 00 00 00 6B 44 4E 49 65 02 01 59 04 21 13 93 00

ATR: 4B 7F 96 00 00 00 6B 44 4E 49 65 00 01 59 04 21 13 93 00
+ TS = 4B --> Direct Convention
+ T0 = 7F, Y(1): 0111, K: 15 (historical bytes)
  TA(1) = 96 --> Fi=512, Di=32, 16 cycles/ETU
    250000 bits/s at 4 MHz, fMax for Fi = 5 MHz => 312500 bits/s
  TB(1) = 00 --> VPP is not electrically connected
  TC(1) = 00 --> Extra guard time: 0
+ Historical bytes: 00 6A 44 4E 50 65 10 02 01 51 04 21 31 90 00
  Category indicator byte: 00 (compact TLV data object)
    Tag: 6, len: A (pre-issuing data)
      Data: 44 02 49 65 10 01 31 55 04
    Mandatory status indicator (3 last bytes)
      LCS (life card cycle): 03 (Initialisation state)
      SW: 9000 (Normal processing.)

Possibly identified card (using /usr/share/pcsc/smartcard_list.txt):
4B 7F 96 6B 00 6B 44 4E 49 65 00 02 01 59 04 21 13 93 00
4B 7F 96 6B 00 00 6B 44 4E 49 65 00 02 01 04 21 13 93 00
        DNIE Spain (eID)
        http://www.dnielectronico.es/PortalDNIe/

   
Mon Aug  4 17:15:46 2025
 Reader 0: Alcor Micro AU9540 00 00
  Event number: 2
  Card state: Card removed, 

```

Try here [DGT puntos](https://sede.dgt.gob.es/es/permisos-de-conducir/permiso-por-puntos/certificado-de-puntos/) 


### autofirma <a name="autofirma"></a>


[URL](https://firmaelectronica.gob.es/content/dam/firmaelectronica/descargas-software/autofirma19/Autofirma_Linux_Debian.zip) Just download the ZIP, unzip and install with dpkg

```
wget https://firmaelectronica.gob.es/content/dam/firmaelectronica/descargas-software/autofirma19/Autofirma_Linux_Debian.zip
unzip Autofirma_Linux_Debian.zip 
sudo dpkg -i autofirma_1_9.deb 
```

## Fingerprint <a name="Fingerprint"></a>

You can use KDE user acount to enroll fingerprints or using the bash command

```
fprintd-enroll

```
To test it
```
fprintd-verify
Using device /net/reactivated/Fprint/Device/0
Listing enrolled fingers:
 - #0: right-index-finger
Verify started!
Verifying: right-index-finger
Verify result: verify-match (done)
```

Now, enable it:

```
sudo pam-auth-update --enable fprintd

```

I prefer to use the fingerprint sensor IF the lid is OPEN, to archieve this I have to modify some files and create a small bash script to detect the lid state.

```
sudo cat /etc/pam.d/common-auth
#
# /etc/pam.d/common-auth - authentication settings common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of the authentication modules that define
# the central authentication scheme for use on the system
# (e.g., /etc/shadow, LDAP, Kerberos, etc.).  The default is to use the
# traditional Unix authentication mechanisms.
#
# As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
# To take advantage of this, it is recommended that you configure any
# local modules either before or after the default block, and use
# pam-auth-update to manage selection of other modules.  See
# pam-auth-update(8) for details.

# here are the per-package modules (the "Primary" block)
auth    [success=ignore default=1]      pam_exec.so quiet /opt/lid.sh # THIS LINE IS KEY, PLACE IT HERE
auth    [success=2 default=ignore]      pam_fprintd.so max-tries=2 timeout=2 # debug
auth    [success=1 default=ignore]      pam_unix.so nullok try_first_pass

```

Now create /opt/lid.sh with this content
```
#!/bin/bash

lid_state="/proc/acpi/button/lid/LID/state"

# Exit with failure if lid is closed, else true
grep -q closed ${lid_state} && exit 1; true
```
```
sudo chmod +x /opt/lid.sh
```

And test it, if equals 0 then the LID is open and 1 the lid is closed.
```
sh /opt/lid.sh 
echo $?
0
```

## Sound <a name="Sound"></a>

Sound can be improved a lot if easyeffects is installed https://github.com/wwmm/easyeffects

```
sudo apt install easyeffects
```

Then you have to create or use a proper profile, I just downloaded json and loaded using easyeffects app [json](https://github.com/sebastian-de/easyeffects-thinkpad-unsuck/raw/refs/heads/main/thinkpad-unsuck.json)

## Resources <a name="Resources"></a>

* [easyeffects-thinkpad-unsuck](https://github.com/sebastian-de/easyeffects-thinkpad-unsuck)
* [psref x390](https://psref.lenovo.com/syspool/Sys/PDF/ThinkPad/ThinkPad_X390/ThinkPad_X390_Spec.PDF)
* [fingerprint authentication](https://wiki.debian.org/SecurityManagement/fingerprint%20authentication)
* [[GUIDE/SOLVED] Sudo and Login with Fingerprint Reader under KDE/Arch Linux](https://community.frame.work/t/guide-solved-sudo-and-login-with-fingerprint-reader-under-kde-arch-linux/37009/21)





