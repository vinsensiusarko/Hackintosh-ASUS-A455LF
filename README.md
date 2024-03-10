# Hackintosh ASUS A455LF Notebook

### Specs :

 - [x] <b>BIOS</b> : v302 (2019/06/04) American Megatrends Inc,
 - [x] <b>Motherboard</b> : X455L
 - [x] <b>Model Laptop/Notebook</b> : Asus A455LF Series
 - [x] <b>Processor</b> : Intel Core i3-5005U (2) @ 2.0GHz Broadwell
 - [x] <b>Graphics</b> : Intel HD Graphics 5500 + Nvidia Geforce 930M
 - [x] <b>RAM</b> : 12 GB 1600 MHz DDR3L (Samsung)
 - [x] <b>SSD</b> : V-Gen 128GB (GUID Partition Table) Disk 0
 - [x] <b>HDD</b> : WD 500GB (GUID Partition Table) with HDD Caddy on Disk 1
 - [x] <b>Audio</b> : Conexant CX20751/2
 - [x] <b>Touchpad</b> : FocalTech PS/2
 - [x] <b>Wifi + BT</b> : ~~Qualcomm Atheros QCA9565/AR9565 Wireless Network Adapter + AR3012 (Azurewave Tech)~~ Replaced by Wireless-AC 7265 Wireless Network Adapter
 - [x] <b>Ethernet</b> : Realtek RTL8111GU/8168GU/8411GU PCI Express Gigabit Ethernet
 - [x] <b>Others</b> : USB3.0 + USB2.0 ports WebCam, ports HDMI/VGA, DVD ROM Matshita, Alcor Micro USB Card Reader, etc.

--------------------------------------------------------------------------------------------

### BIOS Configuration

Bios Config | Setting 
:---:| :---:
Security -> Secure Boot | Disabled
Intel Virtualization    | Enabled OK / Disabled if you have issues
VT-d | Disabled / Enabled OK or boot-args "dart=0" for older Clover
Graphics Configuration -> DVMT Pre-Allocation | 64M / Default 32M (need pre-alloc patches in Device Properties section)
USB Configuration -> XHCI Pre-Boot Mode | Enabled (XHC Only) / Smart Auto (XHC + EHC)
SATA Mode | AHCI
Boot -> Launch CSM | Enabled or Disabled for Resolution Boot OC

--------------------------------------------------------------------------------------------

### What's Working?

 - [x] QE/CI Intel HD Graphics 5500 with VRAM 4095 MB (Cosmetic), Nvidia Geforce 930M (Disable)
 - [x] Audio Conexant CX20751/2 with layout-id 21 + Internal Microphone (SSDT-CX207512.aml + AppleALC.kext)
 - [x] Display brightness PNLF and Fn Keys (Device PNLF taken from MacBook ACPI dump + ASUS DSDT Patches FN Keys + AsusSMC.kext)
 - [x] ~~Qualcomm Atheros AR9565 WiFi (HS80211Family.kext + AirPortAtheros40.kext)~~ Wireless-AC 7265
 - [x] LAN Ethernet Realtek RTL8111GU/8168GU/8411GU (RealtekRTL8111.kext)
 - [x] ~~Bluetooth AR3012 (Ath3kBT.kext + Ath3kBTInjector.kext)~~ Wireless-AC 7265 
 - [x] FocalTech TouchPad PS/2 (ApplePS2SmartTouchpad.kext)
 - [x] Battery Indicator (DSDT RehabMan Battery Laptop Patch for ASUS + SMCBatteryManager.kext)
 - [x] Airdrop + Handoff + Continuity fully supported to Big Sur (Set to true "ExtendBTFeatureFlags" Quirks section in config.plist Clover/OC)
 - [x] TRIM support (Set to true "ThirdPartyDrives" Quirks section in config.plist Clover/OC)
 - [x] USB2.0 Ports + USB3.0 Ports + Power/Speed (Disable XhciPortLimit + USBMap.kext/SSDT-UIAC.aml)
 - [x] Native Power Management CPU (Combination from [ssdtPRGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh) Pike R. Alpha and [ssdt_data](https://github.com/acidanthera/CPUFriend/blob/master/Instructions.md#data-combination) PMheart + CPUFriend.kext + CPUFriendDataProvider.kext)
 - [x] WebCam OOB
 - [x] HDMI, Etc.

--------------------------------------------------------------------------------------------

### Not Working?

 - Nvidia Geforce 930M (Optimus Tech) not supported by hackintosh.

--------------------------------------------------------------------------------------------

### Bugs?

 - in Monterey only one side, Airdrop/Handoff/Continuity from Hackintosh to iPhone doesn't work, but iPhone to Hackintosh is work, because  Apple has been actively working on the Bluetooth stack (bluetoothd) in macOS Monterey, see [issues](https://github.com/acidanthera/bugtracker/issues/1821).
 - This laptop just have single antenna connector, lol. For pairing bluetooth in chipset DW1550 need dual antenna connectors, so maybe work sometimes not work.
 - Sidecar won't showing in System Preferences since this [patch](https://github.com/acidanthera/FeatureUnlock/commit/2bb3c5023a9d8f3227bb6577378e68723c56975e).

--------------------------------------------------------------------------------------------

### Not Tested?

 - Sidecar
 - Universal Control
 - Card Reader

--------------------------------------------------------------------------------------------

### Notes

1. Don't use my patch [DSDT.aml & SSDT.aml](https://github.com/asepms92/Hackintosh-ASUS-A455LF-Notebook/tree/master/CLOVER/EFI/CLOVER/ACPI/patched) if you have different <b>ACPI Tables / BIOS Version & Model / Freq CPU PM</b>.
2. Don't install Monterey if you have chipset Atheros, because Apple fully dropped for Atheros on Monterey. Just install Big Sur and replace the kexts (AirportBrcm*.kext + Brcm*.kext) -> (HS80211Family.kext + Ath3kBT.kext) take it from [here](https://github.com/asepms92/Hackintosh-ASUS-A455LF-Notebook/tree/master/CLOVER/EFI/CLOVER/kexts/Off).
3. I haven't idea for battery when using OC, because so many rename methods in device EC0/BAT0, when rename method battery and landing to [SSDT-BATT.aml](https://github.com/asepms92/Hackintosh-ASUS-A455LF-Notebook/tree/master/OC/EFI/OC/ACPI), the indicator only show to 1%.
4. Config OC for Catalina and older, set (MinDate = -1) & (MinVersion = -1) in 'UEFI' -> 'APFS' section, and take from the [release page](https://github.com/asepms92/Hackintosh-ASUS-A455LF-Notebook/releases/download/v0.0.1/OpenCore_v0.7.7.zip).

<img src="/.gitbook/assets/set-config-oc-for-catalina-and-older.png?raw=true" alt="Set config OC Catalina and older" align="center">

5. For 10.11-11 you need BrcmBluetoothInjector.kext, for 12 (Monterey) you need BluetoolFixup.kext, choose one and not both.
6. You need regenerate serial number for your mac, use [Clover](https://mackie100projects.altervista.org/download-clover-configurator/)[/OC](https://mackie100projects.altervista.org/download-opencore-configurator/) Configurator or [macserial](https://github.com/asepms92/Hackintosh-ASUS-A455LF-Notebook/tree/master/OC/Utilities/macserial) from OC.
7. And many more [apps](https://github.com/asepms92/Hackintosh-ASUS-A455LF-Notebook/tree/master/Tools/Apps).

--------------------------------------------------------------------------------------------

### Special Thanks and Credits to :

- [Apple](https://www.apple.com)
- [Clover](https://github.com/CloverHackyColor)
- [Acidanthera](https://github.com/acidanthera)
- [Rehabman](https://github.com/RehabMan)
- [Mieze](https://github.com/Mieze)
- [EmlyDinesh](https://github.com/EMlyDinEsHMG)
- [AnV](https://github.com/andyvand)
- [Pike R. Alpha](https://github.com/Piker-Alpha)
- [InsanelyMac](https://www.insanelymac.com)
- [Olarila](http://olarila.com)
- [OSXLatitude](https://osxlatitude.com)
- And Other Developers
