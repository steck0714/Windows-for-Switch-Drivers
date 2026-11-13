# Drivers + Drivers2 統合メモ（2026-11-13）

土台: steck0714/Windows-for-Switch-Drivers `main`（d049738）。
取り込み: steck0714/Windows-for-Switch-Drivers2（＝hayato41200/Windows-for-Switch-Drivers `main`、1c08c87）の新しい分。

- 既存ファイルの削除・変更はなし。例外は `README.md` に「Erista vs Lite Wi-Fi」の短い注意を10行挿入しただけ（消せば元通り）。
- 追加: `drivers/SwitchWlan`（Erista用 `switchwlan` 0.6.13）、`drivers/SwitchWifiProbe`（旧0.4.0を `switchwprobe` に改名）、`kits/Switch-Wlan`、`kits/Switch-Wifi-Probe`、`tools/Build-Driver-WiFiCx.ps1`、`docs/Erista-and-Lite-Wifi.md`。
- `app-hayato/`: hayato41200の `app/`（SwitchCenter 0.8.1、SwitchPatcher 0.5.0）。既存の `app/` とは混ぜていない。
- `reference-hayato/`: 既存ファイルのhayato41200版（読むだけ）。
- `kits/Switch-Wlan/Wlan.ps1` は、Lite用 `switchwifi` 0.5以降を削除しないよう修正済み。Liteでは実行しないこと。
- 未ビルド・未検証。詳細は `docs/Erista-and-Lite-Wifi.md`。
