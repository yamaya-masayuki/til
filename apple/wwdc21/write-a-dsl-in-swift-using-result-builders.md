# Write a DSL in Swift using result builders

## What's DSL

- 問題空間を想定して作られたプログラミング言語。
- コードはカスタム部分を指定するだけ。言語は暗黙的動作を追加する。
- しばしば宣言的である。

### 埋め込みDSL

- 通常の言語で書かれたコードにカスタム言語のルールを適用する。
- スタンドアロンのDSLよりもはるかに簡単に実装できる。
- 非DSLコードとの相互運用が可能。
- 既存のツールを利用できる。
- 顧客の学習が少なくて済む。
- SwiftにはDSL機能が組み込まれている。

### いつDSLを作るべき？

- コードが過度に散らかっているとき。
- 何かを説明しているのであって、実行しているわけではないとき。
- 専門家がDSLを使用するとき。
- ライブラリのようにDSLを頻繁に使用するとき。
- 言語には学習曲線があることを思い出したとき。

## How result builders work

![Intro](./write-a-dsl-in-swift-using-result-builders-intro.png)

### Result builders

SE-0289

Xcode 12.5, Swift 5.4

- ドメイン固有における想定される動作を実装するのに役立つ。
- 関数、メソッド、ゲッター、クロージャーに適用される。
- 暗黙のメソッド呼び出しで文をラップし、その結果を使用できるようにする。
- 関数の戻り値を引き継ぐ。
- コンパイル時の機能 - OSバージョンは問題にならない

💡このあたりは動画を見たほうがわかりやすい。

### Guaranteed to work normally

リザルトビルダーの制限。

- リザルトビルダー関数の構文も同じ。
- 通常の場所(normal places)で名前を探す。
- リザルトビルダーは、いくつかの言語キーワードを無効にする。
   - 条件付きで有効になるものもある。
- キーワードが許可されている場合は、通常通りに動作する。

## Designing a DSL

DSLはAPIに似ているが、それだけではない。

- Swiftでアイデアを表現する方法を決める。
- 決めるのは主観的。
- 使用時のわかりやすさを目指す。
- スキルは双方向に伝わります。

### スムージーDSLの目標

💡スムージーDSLとは本セッション内のサンプルDSLのこと。
💡ここではDSLを用いずに(Swiftで)書いた場合と比較した話しである。

より多くの人がスムージーリストを編集できるようにする。

- `hasFreeRecipe` プロパティをなくす。
- スムージーの定義をリストにマージする。
- 成分表の入力をより簡潔にする。
- スムージーの引数リストから`description`と`measuredIngredients`を取り除く。

DSL前のバージョン。

```swift
// Fruta’s Smoothie lists

extension Smoothie {
  static let berryBlue = Smoothie(
    id: "berry-blue",
    title: "Berry Blue",
    description: "Filling and refreshing, this smoothie will fill you with joy!",
    measuredIngredients: [
      MeasuredIngredient(.orange, measurement: Measurement(value: 1.5, unit: .cups)),
      MeasuredIngredient(.blueberry, measurement: Measurement(value: 1, unit: .cups)),
      MeasuredIngredient(.avocado, measurement: Measurement(value: 0.2, unit: .cups))
    ],
    hasFreeRecipe: true
  )

  static let carrotChops = Smoothie(…)
  static let crazyColada = Smoothie(…)
  // Plus 12 more…
}

extension Smoothie {
  private static let allSmoothies: [Smoothie] = [
    .berryBlue,
    .carrotChops,
    .crazyColada,
    // Plus 12 more…
  ]

  static func all(includingPaid: Bool = true) -> [Smoothie] {
    if includingPaid {
      return allSmoothies
    }
        
    logger.log("Free smoothies only")
    return allSmoothies.filter { $0.hasFreeRecipe }
  }
}
```

💡DSL無しから、DSL最終版に至る過程はセッションをみたほうが良い。

DSL最終版。

```swift
// DSL top-level design

@SmoothieArrayBuilder
static func all(includingPaid: Bool = true) -> [Smoothie] {
  Smoothie(id: "berry-blue", title: "Berry Blue") {
    "Filling and refreshing, this smoothie will fill you with joy!"

    Ingredient.orange.measured(with: .cups).scaled(by: 1.5)
    Ingredient.blueberry.measured(with: .cups)
    Ingredient.avocado.measured(with: .cups).scaled(by: 0.2)
  }

  Smoothie(…) { … }

  if includingPaid {
    Smoothie(…) { … }
  } else {
    logger.log("Free smoothies only")
  }
}
```

### 言語設計のヒント

- 目標を立ててからスタートする。
- 前例やインスピレーションを探す。
- 全体との整合性を考える。
- ミスをなくすようにする。
- 多くの可能性を評価する。
- 一番良いと思うものを選ぶ。

## Writing a result builder

💡ハンズオンっぽい進行なのでセッション動画を見よ。

1. スムージーの配列リザルトビルダーの基本

   ```swift
   @resultBuilder
   enum SmoothieArrayBuilder {
     static func buildBlock(_ components: Smoothie...) -> [Smoothie] {
       return components
     }
   }
   ```

2. 簡単な`if`文を持つスムージーの配列リザルトビルダー

   ```swift
   @resultBuilder
   enum SmoothieArrayBuilder {
     static func buildOptional(_ component: [Smoothie]?) -> [Smoothie] {
       return component ?? []
     }
      
     static func buildBlock(_ components: [Smoothie]...) -> [Smoothie] {
       return components.flatMap { $0 }
     }
      
     static func buildExpression(_ expression: Smoothie) -> [Smoothie] {
       return [expression]
     }
   }
   ```

3. `if-else`文を持つスムージーの配列リザルトビルダー

   ```swift
   @resultBuilder
   enum SmoothieArrayBuilder {
     static func buildEither(first component: [Smoothie]) -> [Smoothie] {
       return component
     }
     
     static func buildEither(second component: [Smoothie]) -> [Smoothie] {
       return component
     }
     
     static func buildOptional(_ component: [Smoothie]?) -> [Smoothie] {
       return component ?? []
     }
     
     static func buildBlock(_ components: [Smoothie]...) -> [Smoothie] {
       return components.flatMap { $0 }
     }
     
     static func buildExpression(_ expression: Smoothie) -> [Smoothie] {
       return [expression]
     }
   }
   ```

### リザルトビルダーの効果

- 1つの機能に適用されます
    - 関数やクロージャのネストはできない
- 適用方法
    - 明示的に属性を記述する
    - プロトロコル要求から推測する
    - クロージャ引数からの推測する

### 無効なコードのハンドリング

- クライアントはミスをするもの
    - エラー体験に注意を払おう
- Swiftはエラーを診断するが、それらはSwiftの用語で表現される

リザルトビルダーにエラーメッセージを追加したバージョン。

```swift
extension Smoothie {
  init(id: Smoothie.ID, title: String, @SmoothieBuilder _ makeIngredients: () -> (String, [MeasuredIngredient])) {
    let (description, ingredients) = makeIngredients()
    self.init(id: id, title: title, description: description, measuredIngredients: ingredients)
  }
}

@resultBuilder
enum SmoothieBuilder {
  static func buildBlock(_ description: String, components: MeasuredIngredient...) -> (String, [MeasuredIngredient]) {
    return (description, components)
  }
  
  @available(*, unavailable, message: "first statement of SmoothieBuilder must be its description String")
  static func buildBlock(_ components: MeasuredIngredient...) -> (String, [MeasuredIngredient]) {
    fatalError()
  }
}
```



## まとめ

DSLは複雑なSwiftコードをより綺麗にできる。

リザルトビルダーは、DSLが使用できるように文の結果を捕獲する。

修飾子スタイルのメソッドはリザルトビルダーと相性が良い。
