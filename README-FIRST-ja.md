# hayato41200 → steck0714（Windows-for-Switch-Drivers）取り込みキット（分離版）

作成日: 2026-11-13
基準: steck0714/Windows-for-Switch-Drivers `main`（d049738）／取り込み元: hayato41200 の後れ26コミット分

**steck0714側にすでにあるファイルは1つも変更・上書きしません。** 新しい分だけを、重ならない場所に分けて入れる構成です。

| フォルダ | 中身 | 使い方 |
|---|---|---|
| `add/` | steck0714側に存在しないファイルだけ（24個）。`switchwlan` 0.6.13、旧プローブ（`switchwprobe` に改名）、各キット、`Build-Driver-WiFiCx.ps1`、`docs/Erista-and-Lite-Wifi.md` | リポジトリのルートにそのままコピー。衝突0件を確認済み |
| `app-hayato/` | hayato41200の `app/` を丸ごと（SwitchCenter 0.8.1、SwitchPatcher 0.5.0）。単体で完結 | 既存の `app/` とは別フォルダとして置く。混ぜない |
| `reference-hayato/` | steck0714側にもあるファイルのhayato41200版（Make-Release.ps1など） | 読むだけ。コピーしない |

## 分けた理由

- `drivers/SwitchWifi`（Lite用0.5.7）と、hayato41200の旧 `switchwifi` 0.4.0は同じパス・同じ名前・同じHWIDで中身が違う → 旧版を **`switchwprobe`** に改名して `drivers/SwitchWifiProbe/` へ（サービス名・SYS・INF・レジストリ・プールタグ）。キットも `kits/Switch-Wifi-Probe/` へ。
- `tools/Build-Driver.ps1` はhayato41200版の方が新しい → 既存は触らず、コピーを `Build-Driver-WiFiCx.ps1` として追加。`switchwlan` のビルドだけがこちらを呼ぶ。
- 既存アプリは `Shared\*.cs` をワイルドカードで取り込むため、新しい `.cs` を `app/Shared` に入れると既存ビルドに混ざる → アプリは丸ごと `app-hayato/` に分離。
- README.md は変更していない。詳しい説明は `add/docs/Erista-and-Lite-Wifi.md` にある。READMEに注意を出したい場合は、`README-paste-snippet.md`（短い注意＋リンク）を、READMEの「## Building and installing」の直前に貼る（貼るかどうかは任意）。

## Lite保護（`kits/Switch-Wlan/Wlan.ps1` を修正済み）

上流版は `switchwifi.inf` を無条件で削除する作りでした。この版は `switchwprobe` と「0.5未満の `switchwifi`（旧プローブ）」だけを削除し、Lite用の `switchwifi` 0.5以降は残します（解析不能なら削除しない側に倒れる）。それでも `Install` は同じデバイス用のドライバを追加するので、Lite機では実行しないでください。

## 未検証

- 何もビルドしていない（`switchwlan` はWDK 10.0.26100.0が必要）。
- `Wlan.ps1` の削除ロジックは、読んで確認し、サンプル出力で正規表現を試しただけ。PowerShellでは実行していない。
- Erista実機での動作は、hayato41200側の記録に依存している。
