# Samsung All-In-One DM500A2J-K26L Hackintosh
 
 ## System specification

| Name | Description |
| - | - |
| CPU | Intel Pentium 3558U |
| IGPU | Intel HD Graphics (Haswell) |
| DGPU | Compatible [AMD](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html) / [Nvidia](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/nvidia-gpu.html) Graphics card in mPCIe slot with EXP GDC |
| Ethernet | Realtek RTL8168 Gigabit Ethernet Controller |
| Wi-Fi / Bluetooth | Qualcomm Atheros AR9565 / AR3012 |
| Audio | Realtek HD Audio ALC282 |
| SD Card Reader | *Realtek USB Card Reader RTS5129 (limited support)* |
 
 ## Issues
1. Apple dropped support for Atheros Wi-Fi since Mojave and AR9565 it not natively supported. You can activate Wi-Fi on Mojave and higher OS using IO80211Family.kext and patched AirportAtheros40.kext from High Sierra.
    - The Wi-Fi is very slow.
    - The card is soldered.
    - Continuity features do not work.
2. The SD care reader cannot read any cards.
3. The IGPU Intel HD Graphics (Haskell) does not work well with macOS. Therefore a compatible external graphics card is required and connected via mPCIe. BIOS mod or modifying setup variables with modified Grub Shell is also required to output the video.
4. ALC282 produces noise when the computer boots or shuts down. Disabling sound output by switching to external monitor cuts off the internal sound output whereas internal sound input is preserved.
 
 
 ## BIOS tweak
 ### Modded BIOS
 1. Learn how to flash AMI BIOS. This may brick the computer, so I don’t recommend it unless you understand the consequences and know how to unbrick in case something goes wrong.
 2. [Link to the BIOS image](https://www.bios-mods.com/forum/Thread-Request-Unlock-Advanced-and-Chipset-tabs-on-Samsung-All-In-One-DM500A2J) Thanks to genius239 at bios-mods.com

 ### Using modified Grub Shell
 
 1. Dump the UEFI using the tool provided in this guide [ASUS G701VI: Unlock Hidden BIOS Settings](https://octoperf.com/blog/2018/11/20/asus-g701vi-bios-unlock/), `1. Software Tools` throgh `2.1 Dumping BIOS`.
 2. Follow Dortania's [Fixing CFG Lock](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html) to learn how to find `CFG Lock`, `Primary Display`, and `IGPU Port Configuration` and also the command setup_var.
 3. Unlock MSR 0xE2 Register.
 4. ind `Primary Display` and `IGPU Port Configuration` register and values.
 4. If the DGPU supports UEFI type find set PCIe as `Primary Display`. If it doesn’t, set IGFX as primary.
 5. If you choose to enable IQSV, Always enable IGPU. If you use iMacPro1,1 profile, disable IGPU.
 
 - Note
    - You should disable the quirks that patched the kernel to boot with CFG Lock on: `AppleCpuPmCfgLock` and `AppleXcpmCfgLock`.
    - 

 ## To-Do's
 
 1. Get a better graphics card. Currently GTX550TI which cannot work reliably on High Sierra or higher.
 
 ## Credits
 
 Apple for macOS
 
 The Acidanthera team for OpenCore and kexts
 
 Other great contributors I have yet to mention
