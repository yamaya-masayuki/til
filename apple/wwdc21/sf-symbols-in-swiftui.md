# SF Symbols in SwiftUI

<https://developer.apple.com/wwdc21/10349>

```swift
// System symbol image
Image(systemName: "heart")

// System symbol label
Label("Heart", systemImage: "heart")

// Custom symbol image
Image("queen.heart")

// Custom symbol label
Label("Queen of Hearts", image: "queen.heart")
```

```swift
TabView {
  Text("Cards").tabItem {
    Label("Cards", systemImage: "rectangle.portrait.on.rectangle.portrait.fill")
  }
  Text("Rules").tabItem {
    Label("Rules", systemImage: "character.book.closed.fill")
  }
  Text("Profile").tabItem {
    Label("Profile", systemImage: "person.circle.fill")
  }
  Text("Magic").tabItem {
    Label("Magic", systemImage: "sparkles.fill")
  }
}
```

`.fill` で塗り潰しシンボルになる。

## まとめ

- シンボルを作る
- シンボル修飾子
- 異形
- レンダリングモード
- 前景スタイル
