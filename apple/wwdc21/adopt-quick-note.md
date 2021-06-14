# Adopt Quick Note

<https://developer.apple.com/wwdc21/10264>

## 概要

アプリをQuick Noteにリンクして、コンテンツとノート、ノートとコンテンツをすばやく結びつける方法をご紹介します。

- `NSUserActivity`を介してQuick Noteがアプリのコンテンツを認識してリンクする方法
- APIをアプリに採用する方法
- Quick Noteをサポートするための要件、メリット、機能
- `NSUserActivity`のガイダンスとベストプラクティスを提供する

## UI

Apple Pencilで画面右下から上にスワイプすると、新しいQuick Noteが作成される。

Picture-in-Picutreと同様に、Quick Noteのウィンドウは、デバイスのどのコーナーにも自由に移動することができる。また、ピンチ操作でウィンドウをデフォルトサイズから最小サイズ、最大サイズまで拡大することができる。

メモが終わったら、左上のDoneボタンを押します。

同じメモに追記するためにキープしておきたい場合は、UIを右にスワイプして、画面の横に収納しておくことができる。

Quick Noteに追加したリンクは、タップするとSafariやマップに移動しますが、Quick Noteサジェスチョンを使えば、これらのアプリからメモに戻ることができる。

## 仕組み

`NSUserActivity`オブジェクトは、アプリの状態をキャプチャして、あとで利用するための方法を提供する。ユーザーアクティビィティオブジェクトは、Webページの閲覧やアプリのコンテンツの閲覧など、何が起こっているかについて作成され、各アプリは現在のアクティビティをシステムに登録する。

システムはこれらのアクティビティを取得し、Handoffのような機能に送信します。Quick Noteのリンクは、このシステムを利用しています。登録されたアクティビティは、Handoff、Spotlight、リマインダーに加えて、Quick Noteにも送られる。これが、リンクの追加メニューにリンクが表示される仕組みであり、クイックノートのサジェスチョンのトリガーとなる。

`NSUserActivity`には、アプリのコンテンツをQuick Noteにリンクするための永続的な識別子となる3つのプロパティがすでに用意されている。このエコシステムに参加するには、以下の3つのプロパティのうち、1つ以上を設定する必要があります。

- `targetContentIdentifier`
- `persistentIdentifier`
- `webpageURL`

どのプロパティを選択するかは、サポートしたい機能によって異なります。

- `targetContentIdentifier`

  状態の復元時にも使用される。このプロパティにより、iPad上アプリの優れたマルチタスク性を実現する。

- `persistentIdentifier`

  Spotlightインデックスでアプリのコンテンツを識別するためにも使用される。

- `WebpageURL`

  Handoff時のWebフォールバックにも使用されます。

Quick Noteのリンクが機能するためには、識別子に必要な特性がいくつかある。

- コンテンツに対して一意であること
- すべてのデバイスで機能するようにグローバルであること
- 時間が経過してもそのコンテンツで機能するように安定していること

アクティビティに対してユニークでグローバル、かつ安定した識別子を持つことで、Quick Noteサジェスチョンが可能になる。

## `NSUserActivity` を採用するには

- アプリの`Info.plist`でサポートされているアクティビティを宣言する
    - `NSUserActivityTypes`キーで宣言する、値は前述の3つから選ぶ
- さまざまな時点で画面に表示されている内容を記述するユーザーアクティビティを作成して登録する
- アクティビティの継続を求める着信要求を処理する
    - アクティビティの継続を求めるリクエストは、Quick Noteのリンクや他のデバイスから送られてくることもある

```swift
// Create the NSUserActivity and describe the content or user activity
let activity = NSUserActivity(activityType: "com.myapp.MyActivityType")
activity.title = document.title

// Set one or more of:
//   .targetContentIdentifier
//   .persistentIdentifier
//   .webpageURL
activity.targetContentIdentifier = "uniqueGlobalStableIdentifier"

// Set userInfo to save app-specific state information 
activity.userInfo = ["myKey": …]

// Attach it to a view controller, window, or other responder; let the system make it current when needed
viewController.userActivity = activity
```

`NSUserActivity`はクロスプラットフォームなので、iOSでもmacOSでも動作する。

最後に、着信アクティビティの受信と、アプリ内での状態の復元を処理する。

```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {

  func scene(_ scene: UIScene, willContinueUserActivityWithType userActivityType: String) {
    // show user feedback while waiting for the NSUserActivity to arrive
  }
  
  func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
    // set up view controllers and views to continue the activity
  }
    
  func scene(_ scene: UIScene, didFailToContinueUserActivityWithType userActivityType: String, error: Error) {
    // show error about failing to continue an activity
  }

  …
}
```

リンクがタップされると、Quick Noteはあなたのアプリを起動し、`UIWindowSceneDelegate`のシーン `willContinueUserActivityWithType` メソッドを呼び出す。ここで、アプリがアクティビティを受け取っていることをフィードバックできる。

そしてQuick Noteは、`scene(continue userActivity:)`でアプリにアクティビティを提供する。

macOSアプリの場合は、アプリデリゲートに相当する`application(willContinueUserActivityWithType)`、`application(continue userActivity)`、`application(didFailToContinueUserActivityWithType)`を実装する。

```swift
class AppDelegate: NSObject, NSApplicationDelegate {

  func application(_ application: NSApplication, willContinueUserActivityWithType userActivityType: String) -> Bool {
    // show user feedback while waiting for the NSUserActivity to arrive
    return true
  }
    
  func application(_ application: NSApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([NSUserActivityRestoring]) -> Void) -> Bool {
    // set up view controllers or documents to continue the activity
    return true
  }
    
  func application(_ application: NSApplication, didFailToContinueUserActivityWithType userActivityType: String, error: Error) {
    // show error about failing to continue an activity, if appropriate
  }
    
  …
}
```

`NSUserActivity`の採用については、WWDC 2014のビデオ、"Adopting Handoff on iOS 8 and OS X" を見ること。`NSUserActivity`は他の多くの機能の基盤になっている。

## ベストプラクティス

### タイトル

`title`プロパティは、人間が読める文字列です。

これは、リンク追加メニューに表示される文字列になる。つまり、タイトルはコンテンツの内容をうまく、説明的に表現する必要がある。一般的には、ドキュメントやアイテムのタイトルを直接使用するほうが良い。

### 識別子

デバイス固有のデータの使用は避けるべき。セッションIDや特定のビューイングプロパティなど、一時的な情報は避ける。

長期的に考えると、アプリが保存したUUIDを写真に使用することで、コンテンツが移動した場合でも、Quick Noteのリンクを復元することができる。

URLはアプリのコンテンツを一意に識別することができるが、多くの場合、一時的な状態の情報を含んでおり、前述の識別子のガイドラインに違反している。

Quick Noteは、`webpageURL`よりも`targetContentIdentifier`または`persistentIdentifier`を優先する。`webpageURL`は、ガイドラインに従っていれば、使用してもまったく問題ない。アプリが状態の復元とSpotlightの両方に`NSUserActivity`を使用している場合、`targetContentIdentifier`と`persistentIdentifier`に同じ値を使用すること。アプリを補完するWebサイトがある場合は、2つ目の予備の識別子としてURLを追加する。これにより、アプリがインストールされていない場合、リンクを開くとWebサイトにリダイレクトされる。

### 現アクティビティ

アプリの現在の`NSUserActivity`が最新であることを確認すること。これは、何が起きているかを把握するということ。

ベストプラクティスは、アクティビティの変化を検出する際に、タイトルと識別子のプロパティを正確に保つこと。

アクティビティのインスタンスを再利用することは推奨されない。

サンプルコードで示したように、アクティビティをviewControllersやviewなどのレスポンダにアタッチして、UIKitやAppKitに現在のアプリのアクティビティを処理させる。レスポンダへのアタッチがうまくいかない場合は、適切なタイミングで`becomeCurrent()`と`resignCurrent()`を呼び出すことで、現在のアプリのアクティビティを手動で管理できる。

パフォーマンスを向上させるには、アクティビティの`needsSave`プロパティを利用する。アプリでアクティビティを適切に復元するために、アクティビティが特定のビュープロパティを必要とする場合がある。たとえば、地図を表示するときの位置やズームなど。これらのプロパティはアクティビティと一緒に渡すことができるが、ジェスチャーのたびに更新するとオーバーヘッドが生じる。アクティビティを更新するのではなく、`needsSave`を`true`に設定する。システムがアクティビティをQuick NoteリンクやHandoffに送信する必要があるときには、デリゲートの`userActivityWillSave`が呼び出され、アプリがオンデマンドですべてのプロパティを更新できるようになる。

### 互換性の制御

- アプリが更新されていたらどうするか？

  互換性の処理を準備するために、アプリの古いバージョンからのアクティビティの受信を処理し、新しいバージョンからのアクティビティを受信した場合は潔く失敗するように設計する。

  これには、アクティビティのバージョン管理や、キーがなかったり、理解できないものがあっても許容できるようにするなどの対応が考えられる。
  
- そのコンテンツがもう存在しない場合は？

  アクティビティがリンクしていたコンテンツは、削除されたり移動されたりしている可能性がある。アプリに戻る際には、削除されている場合はエラーメッセージを表示し、コンテンツが移動されている場合はリダイレクトする。
