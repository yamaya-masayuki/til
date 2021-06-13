# AR Quick Look, meet Object Capture

<https://developer.apple.com/wwdc21/10078>

Object Capture、Reality Composer、AR Quick Lookを組み合わせることで、誰もが一から没入型のAR体験を開発できるようになりました。Object Captureは、AR Quick Lookですぐに見ることができる高品質のアセットを作成します。Reality Composerは、複数のアセットを1つのシーンにまとめたり、モデルにインタラクティブ性を持たせることが簡単にできます。

このセッションを最大限に活用するには、過去の以下のセッションを押さえておくこと:

- WWDC19 "Advances in AR Quick Look"
- WWDC20 "Shop online with AR Quick Look"
    - Apple PayやカスタムアクションをWeb上のARと統合する方法がわかる

## Creating 3D Content

## Content best practices

- `Reduced` と `Medium` 設定両方を評価しろ
- テストではデバイスのバリエーションが必要
- ソース画像の品質を確保しろ
    - 鮮鋭な写真を撮ること
    - 写真同士のオーバーラップ率が70%以上であること

詳細設定 `Full and Raw` はAR Quick Lookで見ることは出来ない。コンテンツをAR Quick Lookで扱いたい場合は、詳細設定を`Reduced` または`Medium`にする必要がある。

### `Reduced` 詳細設定

- Web配信に適している
- 複数アセットを表示する場合
- デフォルトの選択として最適

### `Medium` 詳細設定

- 複雑なオブジェクトに適している
- 単一のヒーローアセット
- appに適している

## Integrate AR Quick Look

### app

```swift
// File: MyPreviewController.swift
func presentARQuickLook() {
  let previewController = QLPreviewController()
  previewController.dataSource = self
  present(previewController, animated: true)
}

// MARK: QLPreviewControllerDataSource
func previewController(_ controller: QLPreviewController, previewItemAt index: Int) -> QLPreviewItem {
  let previewItem = ARQuickLookPreviewItem(fileAt: fileURL) // Local file URL
  return previewItem
}
```

### Web

```html
<!-- File: /products/alarm-clock.html -->
<a rel="ar" href="alarm-clock.usdz#allowsContentScaling=0">
  <img src="alarm-clock-thumbnail.jpb">
</a>
```

`rel="ar"`属性指定によりサムネイルにARバッジが付く。
コンテンツスケーリングをオフにする場合は `allowsContentScaling` を `0` にする。

Webサイトに組み込んだ場合、Apple Payや予約などのカスタムアクションをAR Quick Lookで直接表示し、お客様が注文フローの次のステップに進めるようにすることもできます。

## Real-world applications

### Eコマース

AR Quick Lookの最も一般的な使用例のひとつ。AR Quick Lookで3Dモデルを見ることで、お客様は、靴、フォトフレーム、家具などの商品を自分の空間でイメージすることができる。

Object Captureを使えば、小売業者は3Dの経験がなくても、商品の3Dモデルを簡単に作成することができる。

また、Reality Composerを活用して、視聴体験を向上させるための動作を追加することもできる。

### 博物館

歴史的な遺物は保護ケースに入れられていることが多く、あらゆる角度から見ることが困難。

しかし、対象物を3Dで撮影することで、鑑賞者は自分のデバイス上で、その対象物の詳細な仮想表現を見たり、操作したりすることができる。

AR Quick Lookは、遠隔地の博物館を見学できるだけでなく、バーチャルな世界でしか体験できない新しい形の対面式展示を可能にする。

メトロポリタン美術館のゼミのフィギュアのように、3Dコンテンツの説明や目の不自由な人のためにナレーション音声を入れることもできる。

### 教育

教師は、オブジェクトキャプチャーで作成した3Dモデルを活用して、魅力的な授業を行うことができる。また、技術的には即時のフィードバックとインタラクティブ性を提供できるので、生徒は自分のペースで学び、発見することができる。

例えば、Qloneアプリを使えば、子供たちはお気に入りのおもちゃやアートプロジェクトの3Dモデルを作ることができる。便利なビジュアルガイドを提供し、必要に応じて自動的に写真を撮るので、子供たちは自分でオブジェクトをスキャンすることができる。Qloneは、iOSアプリとMacアプリの間でシームレスに統合されたObject Capture技術を取り入れることで、子供たちが作った作品を友達や家族と簡単に共有できる。

