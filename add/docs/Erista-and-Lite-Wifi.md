# Erista vs Lite Wi-Fi (switchwlan / switchwifi / switchwprobe)

Imported from hayato41200/Windows-for-Switch-Drivers `main` (commits up to 2026-11-12), kept **separate** from the Switch Lite code in this repository. Nothing that already existed here was changed.

## Which driver goes where

| Driver | Folder | Target | State |
| --- | --- | --- | --- |
| `switchwifi` 0.5.7.0 | `drivers/SwitchWifi/package/` | Switch Lite (HDH-001 / T214) | existing, owner-confirmed on the Lite |
| `switchwlan` 0.6.13.0 | `drivers/SwitchWlan/` | Original Switch (Erista / T210) | new, WiFiCx; WPA2-Personal connect and data path reported working on the Erista by hayato41200. Not built or tested here |
| `switchwprobe` 0.4.0.0 | `drivers/SwitchWifiProbe/` | Erista, diagnostics only | new; hayato41200's old `switchwifi` 0.4.0 probe, **renamed** so it cannot collide with the Lite `switchwifi` |

All three bind `ACPI\SWOA0D01`. **Install only one per console.**

## What was renamed to keep them apart

| | hayato41200 original | here |
| --- | --- | --- |
| driver / service / SYS / INF | `switchwifi` | `switchwprobe` |
| registry parameters | `Services\switchwifi\Parameters` | `Services\switchwprobe\Parameters` |
| pool tag | `SwWi` | `SwPr` |
| kit folder | `kits/Switch-Wifi` | `kits/Switch-Wifi-Probe` |
| source | `drivers/SwitchWifi/src/switchwifi.c` | `drivers/SwitchWifiProbe/src/switchwprobe.c` |

The firmware folder `System32\drivers\switchwifi` is shared on purpose: it only holds data files (`brcmfmac4356-pcie.bin/.txt`).

## Other separated pieces

- `tools/Build-Driver-WiFiCx.ps1` is hayato41200's newer `Build-Driver.ps1` (C++ / WiFiCx support, `-Wdk`, `-Ntddi`). `drivers/SwitchWlan/build.ps1` calls this copy. The existing `tools/Build-Driver.ps1` is untouched.
- `app-hayato/` is hayato41200's whole `app/` tree (SwitchCenter 0.8.1, SwitchPatcher 0.5.0, shared code). It is self-contained and is **not** merged into `app/`: the existing projects compile `Shared\*.cs` with a wildcard, so new files there would leak into the existing build.
- `reference-hayato/` holds hayato41200's versions of files that already exist here (`release/Make-Release.ps1`, the OTA design note, kit READMEs), for reading only.

## Cautions

- `kits/Switch-Wlan/Wlan.ps1` was changed from the upstream copy: **Install no longer deletes the Lite `switchwifi` 0.5+.** It removes only `switchwprobe` and a pre-0.5 `switchwifi` probe. If `pnputil` prints localized labels the match fails and nothing is deleted.
- Even so, `Wlan.ps1 Install` adds a second driver for the same device. Do not run it on a Lite.
- SwitchCenter 0.8.x in `app-hayato/` can deliver `switchwlan` over the air and copy user-supplied Wi-Fi, Bluetooth and USB firmware. Do not let it install the Wi-Fi driver on a Lite.
- Nothing here was built. `switchwlan` needs WDK 10.0.26100.0 (WiFiCx / NetAdapterCx). The `Wlan.ps1` removal logic was only checked by reading and by a regex test, not run in PowerShell.

Credit: hayato41200 (https://github.com/hayato41200/Windows-for-Switch-Drivers), GPL-2.0.
