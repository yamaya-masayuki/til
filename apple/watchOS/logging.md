# watchOSのログを参照するには

**NOTE**: 実機で試してない。

[sysdiagnose Logging Instructions](https://download.developer.apple.com/iOS/watchOS_Logs/sysdiagnose_Logging_Instructions.pdf)より、

## ロギングを有効にするには

1. Apple Developer Siteからロギング用プロファイルをダウンロード
2. そのプロファイルをAirDropなどでiPhoneに転送
3. iPhoneにそのプロファイルを登録

プロファイルの有効期限は **3日間** です。

## ログを収集するに

1. Apple Watchを充電中にする
2. Apple WatchがiPhoneと接続している状態にする
3. 15分待つ。その間にApple WatchからiPhoneにファイルが転送される。
4. iPhoneからMacにAirDropでファイルを転送する、もしくは同期する。

ログディレクトリのパスやロギングの無効化は上記PDFに書いてある。
