# Samsung All-In-One DM500A2J-K26L Hackintosh
 
 ## System specification
 
 1. Name:   Samsung All-In-One (AIO) DM500A2J-K26L
 2. CPU: Intel Pentium 3558U
 3. Graphics: Intel HD Graphics (Haswell)
 4. Ethernet: Realtek RTL8168 Gigabit Ethernet Controller
 5. Wi-Fi and Bluetooth: Qualcomm Atheros AR9565 / AR3012
 6. TV Tuner Card: Polaris AV Capture in mPCIe
 7. Audio: Realtek HD Audio (ALC282)
 8. SD Card Reader: Realtek 5129 USB Card Reader (RTS5129)
 
 ## Working
 
 1. Native power management with CPU spoofing
 2. Ethernet
 3. Wi-Fi by loading High Sierra's IO80211HostFamily.kext and modified AirportAtheros40.kext via OpenCore's kext injection.
 4. Audio (Currently disabled with class-code of FFFFFFFF because there is noise when turning on and off)
 
 ## Not working
 
 1. The IGPU Intel HD Graphics (Haswell) has one report of working QE/CI which could be fake and for which the support is only coded for 10.12.6, 10.13.6, and 10.14-10.14.3. I will need to get a compatible GPU and connect it via mPCIe which was supplied with the TV tuner card.
 2. The support for the AR3012 bluetooth has been dropped since High Sierra. I will attach a bluetooth dongle on the EXP GDC USB slot.
 3. The SD card reader RTS5129 has no support in macOS. Faking it as the native RTS5138 does not work.
 
 
 ## Unlock MSR 0xE2 Register(CFG Lock)
 
 I could never find an AMI Aptio Utility Setup upgrade for this model, so I decided to dump the currently written UEFI and then find the register offset there to tweak. It worked.
 
 I will point to the guides that I have used to achieve this.
 
 1. Dump the UEFI using the tool provided in this guide [ASUS G701VI: Unlock Hidden BIOS Settings](https://octoperf.com/blog/2018/11/20/asus-g701vi-bios-unlock/), `1. Software Tools` throgh `2.1 Dumping BIOS`.
 2. Follow Dortania's [Fixing CFG Lock](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html).
 
 - Note
    - If you dumped the UEFI correctly, the offset should be at `0x65`. Use `VerifyMsrE2.efi` to verify that MSR 0xE2 register has changed from `locked` to `unlocked`.
    - You should disable the quirks that patched the kernel to boot with CFG Lock on: `AppleCpuPmCfgLock` and `AppleXcpmCfgLock`.
    - You could follow the rest of the `Unlock Hidden BIOS Settings` guide and probably unlock other hidden menus. None of my interest though.
 ## To-Do's
 
 1. Install a compatible dGPU
 2. Fix audio noise
 3. Make HfsPlus.efi function with OpenCore
 
 ## Credits
 
 Apple for macOS
 
 The Acidanthera team for OpenCore and kexts
 
 Other great contributors I have yet to mention
