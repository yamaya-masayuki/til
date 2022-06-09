# InstrumentsのMetal System Traceを使ってアプリケーションをプロファイルする

[Using Metal System Trace in Instruments to Profile Your App | Apple Developer Documentation](https://developer.apple.com/documentation/metal/using_metal_system_trace_in_instruments_to_profile_your_app?language=objc)の勝手訳です。

## パフォーマンス上の問題点を見つける

キャプチャー結果の検証を迅速に行うために、フレームレートが遅かった時間帯に焦点を絞ります。フレームレートの異常は、フレームがまれにスキップされることで発生することもあれば、一貫してフレームレートが低いことで発生することもあります。いずれの場合も、アプリの表示時間に予期せぬ遅延を見つけることで、フレームレートの異常を特定することができます。

たとえば、[図3](https://developer.apple.com/documentation/metal/using_metal_system_trace_in_instruments_to_profile_your_app?language=objc#3118793)のコールアウト1は、完了までに250ミリ秒を要した表示インスタンスをハイライトしています。コールアウト2は、その間にスキップされた垂直同期（vsync）イベントの数を示しています。

**図3** 一つのディスプレイインスタンスにまたがる複数のvsync
![Figure 3](https://docs-assets.developer.apple.com/published/fd7d802052/rendered2x-1619129570.png)
