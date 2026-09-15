# AINAOS

Smallest most capable OS for the **BLU Pure XL** — to prove a 2015 MediaTek flagship can run a modern, minimal, useful system.

Stock: Android 5.1 Lollipop, abandoned. Goal: hack it, replace it, prove the point.

## Target Device: BLU Pure XL (2015)

| Item | Spec |
|---|---|
| Codename / Model | BLU Pure XL |
| Release | September 2015, Discontinued, $349 unlocked |
| SoC | MediaTek MT6795 Helio X10 (28nm) |
| CPU | 8x ARM Cortex-A53 @ 2.0 GHz, 64-bit |
| GPU | Imagination PowerVR G6200 @ 550 MHz |
| RAM | 3 GB LPDDR3 |
| Storage | 64 GB internal + microSDXC up to 64 GB |
| Display | 6.0" Quad HD Super AMOLED 2560x1440, 490ppi, Gorilla Glass 3 |
| Battery | 3500 mAh Li-Po, 9V/2A quick charge |
| Rear cam | 24 MP, 1/2.3", PDAF, dual-tone LED, 4K@30 |
| Front cam | 8 MP AF |
| SIM | Micro-SIM, 4G LTE (AT&T / T-Mobile bands) |
| Audio | 24-bit/192kHz, dual stereo speakers, Smart PA, DTS |
| Extras | Fingerprint (rear), NFC, Wi-Fi a/b/g/n/ac dual-band, BT 4.0, GPS+GLONASS, microUSB 2.0 |
| Dimensions | 164 x 82.2 x 9.6 mm, 202g, aluminium |
| Stock OS | Android 5.1 Lollipop, Kernel 3.10.x MTK |
| Bootloader / Flash | MediaTek Preloader + SP Flash Tool, scatter-based |

Sources: GSMArena, PhoneArena, MediaTek/BLU press (Sep 2015).

Why this phone:
- True 64-bit octa-core with 3GB/64GB — still usable if we strip bloat.
- MTK MT6795 = hard to mainline (Preloader, PowerVR Rogue blob, 3.10 kernel) — perfect proof-of-concept target.
- QHD AMOLED + 3500mAh forces us to be small and power-efficient.

## Goal

1. Replace Android 5.1 with smallest capable system that still does: calls/SMS/LTE, Wi-Fi, camera (at least stills), audio, sensors, and a usable UI or shell.
2. Document every hack so anyone can reproduce with SP Flash Tool + TWRP.
3. Never hard-brick — see `general.md` golden rule.

Non-goals (v1): mainline kernel, Android app compat, fancy desktop.

## Strategy Candidates

- **A: Halium 7.1 + postmarketOS / Ubuntu Touch rootfs** — reuse MTK 3.10 kernel + libhybris, smallest path to LTE/calls.
- **B: Lineage 12.1/13 (MT6795 ports e.g. Redmi Note 2) adapted** — proven X10 device trees exist.
- **C: Buildroot minimal Linux + direct framebuffer** — absolute smallest, but modem/GPU/camera = major work.
- Current lean: **A, fallback B**. C is stretch proof.

Constraints to design around:
- PowerVR G6200 needs Rogue blob — no mainline GPU. Must keep Android container or fbdev.
- MTK Preloader — DO NOT TOUCH. Flash everything else, recover via SP Flash Tool.
- NVRAM/IMEI lives in mmcblk0p2 — back it up first or phone becomes tablet.
- TWRP 2.8.6.0 is known-good recovery for this device.

## Repo Layout

```
AINAOS/
  README.md          — this file
  general.md         — unbrick golden rules, backup, TWRP (read FIRST)
  STUFF/             — local toolkit: MTK VCOM/CDC drivers, SP Flash Tool v6.2216/v6.2404, platform-tools, KingoRoot APK, Mtk Droid Tool, TWRP images — NOT tracked, see STUFF/README.md
  docs/              — (planned) porting notes, partition map, kernel config
  device/blu-pure-xl/— (planned) scatter, defconfig, device tree, fstab
  os/                — (planned) minimal rootfs build
```

## Quick Start (Safe)

1. Read `general.md`.
2. Install MTK VCOM drivers from `STUFF/` on a Windows box.
3. Full ROM readback in SP Flash Tool: `0x0` len `0xE9000000`, save as `pure_xl_stock.bin` x2 drives.
4. `adb shell dd if=/dev/block/mmcblk0p2 of=/sdcard/nvram.img` + `adb pull`.
5. Flash TWRP 2.8.6.0 to recovery once, test Vol Up + Power.
6. Never check `preloader` in SP Flash Tool. Ever.

## Roadmap

- [x] Repo + docs skeleton
- [ ] Partition table dump + scatter file capture
- [ ] Stock boot.img / recovery.img extraction + kernel version confirm
- [ ] Pick base: Halium 7.1 vs Lineage 13 port
- [ ] Minimal boot: kernel + initramfs + telnet, then modem, then UI
- [ ] Size budget: define "smallest capable" (e.g. <500MB rootfs target)

## Contributing

PRs welcome. No preloader.bin in PRs. No IMEI/NVRAM dumps in PRs.
