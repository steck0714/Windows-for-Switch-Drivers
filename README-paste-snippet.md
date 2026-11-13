## Erista vs Lite Wi-Fi

> **Switch Lite owners: keep `switchwifi`.** The Wi-Fi code imported from hayato41200 (`switchwlan`, and his old probe, renamed `switchwprobe`) targets the original Switch (Erista). It binds the same device (`ACPI\SWOA0D01`) as `switchwifi`, so install only one Wi-Fi driver per console.

- Do not run `kits/Switch-Wlan/Wlan.ps1` (Install) on a Lite. It adds a second driver for the same device. It leaves `switchwifi` 0.5 and later in place, but the two drivers still compete.
- Do not let SwitchCenter from `app-hayato/` install the Wi-Fi driver on a Lite.
- The imported code has not been built or tested in this repository.

Details and the list of renamed files: [docs/Erista-and-Lite-Wifi.md](docs/Erista-and-Lite-Wifi.md).

