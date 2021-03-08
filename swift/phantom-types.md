# Swiftに於ける幽霊型

[Phantom types in Swift | Swift with Majid](https://swiftwithmajid.com/2021/02/18/phantom-types-in-swift/)の勝手訳。

静的型システムを持つすべての言語がSwiftのような強力な型安全性を持っているわけではありません。
Swiftの幽霊型、汎用型拡張、関連型を持つ列挙型のような機能は、優れた基盤を作っています。今週は、型安全なAPIを構築するために幽霊型を使用する方法を学びます。

## 基本

幽霊型とは、宣言されているが、宣言されている型の内部では決して使用されない汎用型のことです。
通常は、より型安全で堅牢なAPIを構築するための一般的な制約として使用されます。簡単な例を見てみましょう。

```swift
struct Identifier<Holder> {
    let value: Int
}
```

上の例では、`Identifier` 構造体に汎用の Holder 型が宣言されています。ご覧のように、Identifier型の中ではHolder型を使用していません。そのため、幽霊型と呼ばれています。では、このように幽霊型を使うメリットを考えてみましょう。

```swift
struct User {
    let id: Identifier<Self>
}

struct Product {
    let id: Identifier<Self>
}

let product = Product(id: .init(value: 1))
let user = User(id: .init(value: 1))

user.id == product.id
```

`User`型と`Product`型を作成し、先に作成した`Identifier`構造体を使用します。新しく作成した`user`と`product`に対して、識別子の値を1に設定します。しかし、それらを比較しようとすると、Swiftコンパイラはエラーで失敗します:

```text
Binary operator ‘==’ cannot be applied to operands of type ‘Identifier-User’ and ‘Identifier-Product’.
```

そして、`user`識別子と`product`識別子を比較する理由がないので、それは素晴らしいことです。偶然にしかできません。Swiftコンパイラは、幽霊型のために`user`と`product`の間で識別子を混ぜることができず、全く異なる型として認識してしまいます。ここでは、Swiftコンパイラが識別子を混ぜることを許可していない別の例を示します。

```swift
func fetch(_ product: Identifier<Product>) -> Product? {
    // return product by id
}

fetch(user.id)
```

> 幽霊型を用いる利点について、より知りたい場合は[Building type-safe networking in Swift](https://swiftwithmajid.com/2021/02/10/building-type-safe-networking-in-swift/)を参照のこと。

## HealthKitに於ける型安全

幽霊型の基本を学びました。今度はより高度な例に移ることができます。

私はHealthKitを使用して、Apple Watchから健康データを保存してクエリする健康志向のアプリをいくつか作ってみました。Apple Healthアプリからデータを取得するために使用している典型的なコードを見てみましょう。

```swift
import HealthKit

let store = HKHealthStore()
let bodyMass = HKQuantityType.quantityType(
    forIdentifier: HKQuantityTypeIdentifier.bodyMass
)!
let query = HKStatisticsQuery(
    quantityType: bodyMass,
    quantitySamplePredicate: nil,
    options: .discreteAverage
) { _, statistics, _ in
    let average = statistics?.averageQuantity()
    let mass = average?.doubleValue(for: .meter())
}

store.execute(query)
```

上の例では、Apple Healthアプリからユーザーの体重を取得するクエリを作成します。完了ハンドラでは、平均値を取得し、それをメートルに変換しようとしています。

お察しの通り、体重をメートルに変換することは不可能で、ここでアプリがクラッシュしてしまいます。今後は幽霊型を導入して、より型安全なAPIを構築することで解決していきたいと思います。

```swift
enum Distance {
    case mile
    case meter
}

enum Mass {
    case pound
    case gram
    case ounce
}

struct Statistics<Unit> {
    let value: Double
}

extension Statistics where Unit == Mass {
    func convert(to unit: Mass) -> Double {

    }
}

extension Statistics where Unit == Distance {
    func convert(to unit: Distance) -> Double {

    }
}

let weight = Statistics<Mass>(value: 75)
weight.convert(to: Distance.meter)
```

APIの安全性を向上させるために幽霊型を使用するHealthKitフレームワークで考えられる解決策を紹介します。

`Mass`と`Distance`の列挙型を導入して単位を区別するようにしています。そして、質量を距離に変換しようとした途端に、Swiftコンパイラが大きなエラーメッセージを出して止めてしまいます:

```text
Cannot convert the value of type ‘Distance’ to expected argument type ‘Mass’
```

## まとめ

今日はSwift言語の中でも特に好きな機能の一つである幽霊型について学びました。幽霊型の応用例はたくさんありそうです。幽霊型を使ってAPIをより型安全にする方法を私に共有してください。

この記事を楽しんでいただけたら幸いです。

この記事に関連する質問は[Twitter](https://twitter.com/mecid)でフォローしてください。ご愛読ありがとうございました！また来週お会いしましょう

## 訳者注

C++のタグディスパッチみたいだなと思ったら、[本当にそうらしい](https://faithandbrave.hateblo.jp/entry/20121205/1354690830)。
