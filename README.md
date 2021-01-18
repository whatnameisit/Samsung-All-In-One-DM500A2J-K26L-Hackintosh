# Samsung ATIV One 5 DM500A2J-K26L Hackintosh

## System specification
| Name | Description |
| - | - |
| Model | Samsung ATIV One 5 DM500A2J-K26L |
| CPU | Intel Pentium 3558U |
| IGPU | Intel HD Graphics (Haswell) |
| DGPU | Compatible [AMD](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html) / [Nvidia](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/nvidia-gpu.html) Graphics card in mPCIe slot with EXP GDC + half-to-full mPCIe extension card |
| Ethernet | Realtek RTL8168 Gigabit Ethernet Controller |
| Wi-Fi / Bluetooth | Qualcomm Atheros AR9565 / AR3012 |
| Audio | Realtek HD Audio ALC282 |
| SD Card Reader | *Realtek USB Card Reader RTS5129 (limited support)* |
 
## Issues
1. Apple dropped support for Atheros Wi-Fi since Mojave and AR9565 it not natively supported. You can activate Wi-Fi on Mojave and higher OS using IO80211Family.kext and patched AirportAtheros40.kext from High Sierra.
    - The Wi-Fi is very slow.
    - Continuity features do not work.
    - The card is soldered and cannot be replaced.
2. The SD care reader cannot read any cards.
3. The IGPU Intel HD Graphics (Haswell) does not work well with macOS. Therefore a compatible external graphics card is required and connected via mPCIe. Modding UEFI or setup variables with modified Grub Shell is also required to make UEFI recognize DGPU.
4. On non-Windows, ALC282 produces noise when the computer boots or shuts down. Disabling sound output by switching to external monitor cuts off the internal sound output whereas internal sound input is still preserved.
5. The fan on the graphics card does not sleep sometimes.
 
## UEFI setup tweaks
### Using modded Aptio Setup Utility image
1. Learn how to flash AMI UEFI. This may brick the computer, so I don’t recommend it unless you understand the consequences and know how to unbrick in case something goes wrong.
2. [Link to the UEFI mod work](https://www.bios-mods.com/forum/Thread-Request-Unlock-Advanced-and-Chipset-tabs-on-Samsung-All-In-One-DM500A2J) Thanks to genius239 at bios-mods.com
3. Use [this](https://www.supermicro.com/en/products/motherboard/X10SLQ-L) version of Afudos which still has /GAN option to force flash.

### Using modified Grub Shell
1. Dump the UEFI using the tool provided in this guide [ASUS G701VI: Unlock Hidden BIOS Settings](https://octoperf.com/blog/2018/11/20/asus-g701vi-bios-unlock/), `1. Software Tools` throgh `2.1 Dumping BIOS`.
2. Follow Dortania's [Fixing CFG Lock](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html) to learn how to find `CFG Lock` register offset and use `setup_var`. You can use the same method to also find `Primary Display` and `IGPU Port Configuration` offsets.
4. Find `CFG LOCK`, `Primary Display`, and `IGPU Port Configuration` offsets and setup values.
4. If the DGPU supports UEFI, set `PCIe` as `Primary Display`. If it doesn’t, set `IGFX` as primary.
    - You will need to switch the monitor's `Source` and set `PC` mode to get into UEFI setup and OpenCore bootpicker if `IGFX` is primary.
5. If you choose to enable IQSV, always enable `IGFX`. If you use iMacPro1,1 profile, disable `IGFX`.
- Note
    - You should disable the quirks `AppleCpuPmCfgLock` and `AppleXcpmCfgLock` now the UEFI allows updating MSR 0xE2 register.
    
## SMBIOS choice
I am currently using iMac14,2 profile to boot into El Capitan which does not support iMacPro1,1. `SSDT-USBX.aml` is not needed as Apple specifies USB power for iMac14,2 in stock kexts. Once I get a better graphics card and use iMacPro1,1 profile, I will need to add the SSDT.

## To-Do's
Get a better graphics card. Currently GTX550TI which cannot work reliably on High Sierra or higher.

## Others
If you have a variant such as DM500A2J-K30D, K32D, or K38D, you will notive that the CPU is an i3 model. Lucky you. A lot of fun things could be done.
- Delete the kernel patch `Fake CPUID` and enjoy native power management and advanced CPU features.
- Inject a working `ig-platform-id` and `SSDT-PNLF` found in OpenCorePkg bundle for working iGPU QE/CI and native brightness control. Test the HDMI-out and configure the framebuffer.
- Change `SMBIOS` to a iMac14,4 which is an iGPU-only model. You can update to Big Sur with no problem. You still have DRM issues.
- Buy a Broadcom Wi-Fi/Bluetooth Combo module for mPCIe. There are three antennas on the mainboard: Atheros AR9565 / AR3012 has two antennas and TV Tuner Card originally on mPCIe slot has one antenna. Their form factor is U.FL. One option would be BCM94352HMB and Atheros's two antennas. The antennas may be short; use your soldering skills or tear down the motherboard to connect them. Another option is BCM94360HMB and three MHF4 to U.FL adaptors that connect to all three antennas. Test the signal and reposition the TV Tuner Card's antenna which is connected to the TV coax socket. BCM94352HMB works with AirportBrcmFixup and BrcmPatchRAM, and BCM94360HMB works natively. Both support Continuity. And disable Atheros AR9565 / AR3012 by injecting `class-code=FFFFFFFF` into AR9565 and killing AR3012 USB port.

## Credits
Apple for macOS

The Acidanthera team for OpenCore and kexts

Other great contributors I have yet to mention
