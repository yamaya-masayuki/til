# What's new in SwiftUI

<https://developer.apple.com/wwdc21/10018>

- `.refreshable` 修飾子

```swift
.refreshable {
    await photoStore.update()
}
```

- `.task` 修飾子

`AsyncSequence` をイテレートする例:

```swift
.task {
  for await photo in photoStore.newestPhotos {
    photoStore.push(photo)
  }
}
```
