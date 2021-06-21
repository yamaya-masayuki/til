# WWDC 2021 視聴ガイド

[WWDC 2021 Viewing Guide](https://useyourloaf.com/blog/wwdc-2021-viewing-guide/) の勝手訳。

## 何から始めればいい？

新機能をまとめた[Platforms State of the Union (SOTU)](https://developer.apple.com/videos/play/wwdc2021/102/)をご覧ください。

- Xcode Cloudのデモ
- Swiftのコンカレンシーの変更の紹介
- SwiftUIのアップデートの紹介

などがあります。

## 今日中に新しいことをすべて学ぶ必要はありません

膨大な量の新しいものを学ぶことができます。自分が取り残されているように感じるかもしれません。それに押しつぶされることはありません。時間はあります。

新しいAPIの多くはiOS 15を必要とするので、使えるようになるまでにはしばらく時間がかかるかもしれません。**追いつこうとして疲れてしまわないようにしましょう。**

- [Meditation for fidgety skeptics - WWDC 2021 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2021/10316)が助けになるかもしれません。

## セッションの視聴について

私は、Apple Developerアプリを使ってビデオを見ています。このアプリは、macOSとiOSに対応しています。ビデオプレーヤーは0.5倍から2倍までの再生に対応しており、多くのビデオにはトランスクリプトがあり、画面上のサンプルコードのコピーにも対応しています。

セッション数は多いですが、10～20分程度の短いものが多いです。

## Swift

Swiftプロジェクトがオープンソースであることから、これらのセッションはあまり期待できませんが、それでも必見です。

- [What's new in Swift](https://developer.apple.com/videos/play/wwdc2021/10192) Swiftで何が起こっているのか、Swift 5.5での変更点をまとめています。

### Swift Concurrency

Swift Concurrencyとasync/await/actorsは今年の大きな特徴の一つです。この最初のセッションは必見ですが、[Swift ConcurrencyにはiOS 15が必要](https://forums.swift.org/t/will-swift-concurrency-deploy-back-to-older-oss/49370)であることに注意。

- [Meet async/await in Swift](https://developer.apple.com/videos/play/wwdc2021/10132) 私は、async/awaitにまつわる興奮を少し理解していませんでした。このセッションで、その意味を理解することができました。

何度も見返してしまうような深いセッション。

- [Explore structured concurrency in Swift](https://developer.apple.com/videos/play/wwdc2021/10134)
- [Protect mutable state with Swift actors](https://developer.apple.com/videos/play/wwdc2021/10133)
- [Meet AsyncSequence](https://developer.apple.com/videos/play/wwdc2021/10058)
- [Swift concurrency: Behind the scenes](https://developer.apple.com/videos/play/wwdc2021/10254)

理論は素晴らしいけど、どうやって使うの？

- [Use async/await with URLSession](https://developer.apple.com/videos/play/wwdc2021/10095)
- [Swift concurrency: Update a sample app](https://developer.apple.com/videos/play/wwdc2021/10194) Ben Cohen氏によるリファクタリングセッションをお見逃しなく。

### その他の興味深いSwiftセッション

- [Write a DSL in Swift using result builders](https://developer.apple.com/videos/play/wwdc2021/10253) 独自のDSLを作成するためのリザルトビルダーの使用に関する素晴らしいセッションです。
- [Discover and curate Swift Packages using Collections](https://developer.apple.com/videos/play/wwdc2021/10197) Swift Packagesのコレクションを作成、共有します。
- [Meet the Swift Algorithms and Collections packages](https://developer.apple.com/videos/play/wwdc2021/10256) オープンソースのSwiftパッケージとして、これらのSwift標準ライブラリを入手する方法が気に入っています。
- [ARC in Swift: Basics and beyond](https://developer.apple.com/videos/play/wwdc2021/10216) 観測されたオブジェクトの寿命に依存してはいけません。オブジェクトの寿命を最適化するための実験的なビルド設定。

## SwiftUI

SwiftUIの3回目のリリースは、多くの粗削りな部分が取り除かれ、ギャップが埋められた堅実なアップデートです。

- [What’s new in SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10018/) 多くの変更点の概要。非同期画像読み込み、非同期タスク・モディファイア、リストセパレータのカスタマイズ、スワイプアクション、macOSでのテーブル、検索、描画用キャンバス、Markdownテキストのサポート、フォーカスとボタンのスタイルなど。視聴必須。

詳細なセッションはこちら。

- [Demystify SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10022) 視聴必須。SwiftUIのレイアウトについて、こういうセッションがあってもいいと思う。
- [Discover concurrency in SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10019) Concurrencyを使う方法についての素晴らしいセッション。
- [Craft search experiences in SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10176) もう自分で検索バーを回す必要はありません。検索候補をサポートするクロスプラットフォームの検索可能なモディファイアです。これは素晴らしいですね。
- [Add rich graphics to your SwiftUI app](https://developer.apple.com/videos/play/wwdc2021/10021) 安全な領域を用いる、前景スタイル、マテリアルとキャンバスビューでの描画。
- [SF Symbols in SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10349) Symbols 3へのアップデートに対応した新しいモディファイアとレンダリングモード。
- [Direct and reflect focus in SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10023) フォーカスを制御する。

## UIKit

UIKitもお忘れなく。ここにも便利な改良が加えられています。

- [What’s new in UIKit](https://developer.apple.com/videos/play/wwdc2021/10059) まずは何が変わったのかを簡単にまとめたもの。

詳しいセッションはこちら。

- [Meet the UIKit button system](https://developer.apple.com/videos/play/wwdc2021/10064) ボタンは、より簡単なカスタマイズ、ダイナミックタイプ、マルチラインのサポートにより、ボタンらしく戻りました。
- [Customize and resize sheets in UIKit](https://developer.apple.com/videos/play/wwdc2021/10063) スクリーン1/2サイズと非モーダルシート。
- [Your guide to keyboard layout](https://developer.apple.com/videos/play/wwdc2021/10259) 新しいAuto Layoutキーボードレイアウトガイドでは、ビューをキーボードから遠ざけたり、ドッキングされていないキーボードがエッジに近づいたり遠ざかったりしたときに制約を切り替えることができます。
- [Make blazing fast lists and collection views](https://developer.apple.com/videos/play/wwdc2021/10252) プリフェッチとセルの再構成を使用するには。
- [SF Symbols in UIKit and AppKit](https://developer.apple.com/videos/play/wwdc2021/10251) UIKitとAppKitで新しいSFシンボルを使用するには。

## iPadOS

今年はキーボードに大注目。

- [Take your iPad apps to the next level](https://developer.apple.com/videos/play/wwdc2021/10057) iPadのウィンドウサポートを拡張し、ユーザーが特定のコンテンツを新しいウィンドウシーンで開くことができるようになりました。UIMenuBuilderを使ったキーボードショートカットの新しいインターフェイス。ポインターの強化。
- [Focus on iPad Keyboard navigation](https://developer.apple.com/videos/play/wwdc2021/10260) フォーカスをコントロールして、キーボードナビゲーションをカスタマイズ。

## MacとCatalyst

- [What’s new in AppKit](https://developer.apple.com/videos/play/wwdc2021/10054) には、SF Symbols 3、TextKit 2、Swift Concurrency APIのアップデートも含まれています。ビュー無効化のための新しいプロパティラッパーは、いくつかの不要な部分を取り除きました。
- [Meet Shortcuts for macOS](https://developer.apple.com/videos/play/wwdc2021/10232) がMacに登場しました。従来のAutomatorワークフローの移行ツールも含まれています。

iOSアプリケーションをMacで動かすためのさまざまな方法。

- [Qualities of great iPad and iPhone apps on Macs with M1](https://developer.apple.com/videos/play/wwdc2021/10056) M1を搭載したMacでアプリケーションが動作する場合は、App Store Connectの「互換性の検証」リンクを使用して、App Storeの「Not verified for macOS」を「Designed for iPad」に置き換えてください。
- [What’s new in Mac Catalyst](https://developer.apple.com/videos/play/wwdc2021/10052) ボタン、ツールチップ、印刷、ウィンドウの字幕のための新しいAPIです。
- [Qualities of a great Mac Catalyst app](https://developer.apple.com/videos/play/wwdc2021/10053) iPadアプリにCatalystサポートを追加することを振り返る。

Macアプリを作る、コードに沿って解説する2部構成。SwiftUIを使用することで、iOS開発者にとっては馴染みのある分野です。

- [SwiftUI on the Mac: Build the fundamentals](https://developer.apple.com/videos/play/wwdc2021/10062)
- [SwiftUI on the Mac: The finishing touches](https://developer.apple.com/videos/play/wwdc2021/10289)

## WatchOS

アプリの常時表示が今年の大きな特徴。

- [What’s new in watchOS 8](https://developer.apple.com/videos/play/wwdc2021/10002) アプリのための常時接続ディスプレイ。HealthKitデータとBluetooth接続のバックグラウンド配信。地域ベースのユーザー通知。
- [There and back again: Data transfer on Apple Watch](https://developer.apple.com/videos/play/wwdc2021/10003) Apple Watchとの間でデータを転送するためのさまざまな方法。まとめの表は最後までお読みください。
- [Connect Bluetooth devices to Apple Watch](https://developer.apple.com/videos/play/wwdc2021/10005) バックグラウンドアプリの更新時にBluetoothデバイスのデータを取得する方法の詳細です。
- [Build a workout app for Apple Watch](https://developer.apple.com/videos/play/wwdc2021/10009) HealthKitデータと常時表示を利用する。

## Developer Tools

Xcode 13では、Xcode Cloudが大きなニュースですが、新しいドキュメント・コンパイラや多くの改善点もあります。

### Xcode Cloud

- [Meet Xcode Cloud](https://developer.apple.com/videos/play/wwdc2021/10267) ビルド、分析、テスト、アーカイブ、配布を行います。ベータ版にサインアップしてお試しください。
- [Explore Xcode Cloud workflows](https://developer.apple.com/videos/play/wwdc2021/10268) ワークフローのカスタマイズといくつかの推奨ワークフローを紹介します。
- [Customize your advanced Xcode Cloud workflows](https://developer.apple.com/videos/play/wwdc2021-10269) シークレット、カスタムスクリプト、追加リポジトリ、Webhook用の環境変数です。

### DocC - Documentation Compiler

これは、Xcodeのドキュメンテーションツールに大きな改善をもたらします。

- [Meet DocC documentation in Xcode](https://developer.apple.com/videos/play/wwdc2021/10166) これは素晴らしい。リファレンスドキュメント、記事、チュートリアルを作成し、それらをドキュメントブラウザに表示したり、Web上でホストすることができます。
- [Host and automate your DocC documentation](https://developer.apple.com/videos/play/wwdc2021/10236) Webサーバー上でDocCアーカイブをホスティングする方法。DocCアーカイブの構築と配布を自動化します。
- [Elevate your DocC documentation in Xcode](https://developer.apple.com/videos/play/wwdc2021/10167) 基本的なシンボルやメソッドのリファレンスドキュメントを超えて、ドキュメントカタログや記事を追加します。
- [Build interactive tutorials using DocC](https://developer.apple.com/videos/play/wwdc2021/10235) Apple SwiftUI landmarksチュートリアルのようなインタラクティブなチュートリアルを作成できます。

### Interface Builder

- [Build interfaces with style](https://developer.apple.com/videos/play/wwdc2021/10196) 最後かも... アウトラインビューでストーリーボードのシーンを並べ替えたり、制約のグループのプロパティを編集することができます。新しいボタンとテーブルビューのセルコンテンツのスタイル、SFシンボルとアクセシビリティインスペクタのサポート。

### Localization

- [Localize your SwiftUI app](https://developer.apple.com/videos/play/wwdc2021/10220) キーボードのショートカットは、異なるキーボードで動作するように翻訳されます。文字列を抽出するためにコンパイラを使用するように既存のプロジェクトをオプトイン。
- [Streamline your localized strings](https://developer.apple.com/videos/play/wwdc2021/10221) Xcode 13では、エクスポートされたローカリゼーションカタログを直接表示および編集できます。文法の自動一致（英語とスペイン語）。

### Source Control

- [Review code and collaborate in Xcode](https://developer.apple.com/videos/play/wwdc2021/10205) GitHubやBitBucketのプルリクエストワークフローで共同作業を行なうことができます。

### Testing

- [Meet TestFlight on Mac](https://developer.apple.com/videos/play/wwdc2021/10170) 2021年秋に登場。
- [Diagnose unreliable code with test repetitions](https://developer.apple.com/videos/play/wwdc2021/10296) Xcode 13では、失敗するまで実行する、または失敗したら再試行するテストの繰り返しモードが追加されました。失敗時に一時停止してデバッガでキャッチすることも可能。
- [Embrace Expected Failures in XCTest](https://developer.apple.com/videos/play/wwdc2021/10207) 期待される失敗を追跡するためにXCTExpectFailureを使用します。
- [Explore Digital Crown, Trackpad, and iPad pointer automation](https://developer.apple.com/videos/play/wwdc2021/10208)  iPadのポインター、デジタルクラウンの回転、macOSのトラックパッドのサポートをテストし、スワイプしてサイドバーを表示するなどのテストができます。

### Performance and Debugging

- [Discover breakpoint improvements](https://developer.apple.com/videos/play/wwdc2021/10209) カラムブレークポイントは、必要性に気づかなかった機能です。
- [Analyze HTTP traffic in Instruments](https://developer.apple.com/videos/play/wwdc2021/10212) 新しいHTTPインスツルメンツです。これは便利そうですね。
- [Ultimate application performance survival guide](https://developer.apple.com/videos/play/wwdc2021/10181) アプリのパフォーマンス問題を発見するために使用できる数多くのツールをご紹介します。
- [Diagnose Power and Performance regressions in your app](https://developer.apple.com/videos/play/wwdc2021/10087) オーガナイザーを使ってパフォーマンスの後退を探します。
- [Detect and diagnose memory issues](https://developer.apple.com/videos/play/wwdc2021/10180) メモリテストのリグレッション（使用率、リーク、フラグメンテーション）を調査するための新しい診断方法です。
- [Detect bugs early with the static analyzer](https://developer.apple.com/videos/play/wwdc2021/10202) スタティック・アナライザーが、無限ループ、未使用／冗長コード、アサートの副作用をチェックするようになりました。
- [Understand and eliminate hangs from your app](https://developer.apple.com/videos/play/wwdc2021/10258)
- [Triage TestFlight crashes in Xcode Organizer](https://developer.apple.com/videos/play/wwdc2021/10203) TestFlightのクラッシュフィードバックがXcodeのクラッシュオーガナイザーに含まれるようになりました。
- [Explore advanced project configuration in Xcode](https://developer.apple.com/videos/play/wwdc2021/10210)
- [Symbolication: Beyond the basics](https://developer.apple.com/videos/play/wwdc2021/10211) 深く掘り下げます。

## System Frameworks

### Foundation

- [What’s new in Foundation](https://developer.apple.com/videos/play/wwdc2021/10109) インラインのMarkdownスタイルをサポートする、まったく新しいSwift AttributedStringです。フォーマッタが書き直され、フォーマッタを作成する必要がなくなりました。ローカライズされた文字列での文法の一致（スペイン語と英語のみ）。

### CloudKit

- [What’s new in CloudKit](https://developer.apple.com/videos/play/wwdc2021/10086) 非同期バージョンのAPI、暗号化されたフィールド、ゾーンを使ったより詳細な共有。
- [Automate CloudKit tests with cktool and declarative schema](https://developer.apple.com/videos/play/wwdc2021/10118) 新しいスキーマ言語とcktoolコマンドラインツールは、テストを実行する前にコンテナを設定するプリランスクリプトとして使用できます。
- [Meet CloudKit Console](https://developer.apple.com/videos/play/wwdc2021/10117) デザインを一新したコンソールです。

### Location

- [Meet the Location Button](https://developer.apple.com/videos/play/wwdc2021/10102) 毎回プロンプトを出さずに"allow-once"のロケーション認証を取得できます。

### Core Data

- [Bring Core Data concurrency to Swift and SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10017) performとperformAndWaitの非同期バージョン。SwiftUIは動的なフェッチリクエストの設定とセクションフェッチリクエストを取得します。
- [Showcase app data in Spotlight](https://developer.apple.com/videos/play/wwdc2021/10098) SpotlightでCore Dataをインデックス化するのが非常に簡単になりました。履歴追跡を有効にしたSQLiteストアが必要です。
- [Build apps that share data through CloudKit and Core Data](https://developer.apple.com/videos/play/wwdc2021/10015) この機能がSOTUで言及されなかったのは驚きです。プライベートストアをCloudKitにミラーリングし、ユーザー間でレコードを共有することを容易にする新しいAPIです。

### Notifications

- [Send communication and Time Sensitive notifications](https://developer.apple.com/videos/play/wwdc2021/10091) 新しい通知管理では、通知の中断レベルを設定して、通知の動作を制御することができます。

### Networking

- [Reduce network delays for your app](https://developer.apple.com/videos/play/wwdc2021/10239) 新しい開発者向けの設定で、動作環境下でのネットワーク応答性をテストできます（単純なスピードテスト以上）。Appleが光の速度を改善できないのはがっかりです。
- [Accelerate networking with HTTP/3 and QUIC](https://developer.apple.com/videos/play/wwdc2021/10094) iOS 15ではデフォルトでオンになっており、TCPに代わってQUICが採用されています。新しいHTTP計測器で確認。
- [Optimize for 5G networks](https://developer.apple.com/videos/play/wwdc2021/10103)

### Other Services

- [Adopt Quick Note](https://developer.apple.com/videos/play/wwdc2021/10264/) SUserActivityを使ってアプリのコンテンツとノートを連携させます。
- [Build Mail app extensions](https://developer.apple.com/videos/play/wwdc2021/10168) メールの拡張機能を構築するための新しいMailKitフレームワークです。プラグインに代わるものです。
- [Meet TextKit 2](https://developer.apple.com/videos/play/wwdc2021/10061) 新しいテキストエンジン。UITextFieldは自動的にTextKit 2を使用します。
- [Get ready for iCloud private relay](https://developer.apple.com/videos/play/wwdc2021/10096) iCloud+は、Safariのブラウズ、DNSクエリ、およびアプリケーショントラフィックのサブセットをプロキシサーバー経由で転送します。
- [Meet the Screen Time API](https://developer.apple.com/videos/play/wwdc2021/10123) あなたのアプリケーションにScreen Timeとペアレンタルコントロールを追加しましょう。
