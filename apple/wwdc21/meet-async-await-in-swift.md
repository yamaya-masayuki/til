# Meet async/await in Swift

<https://developer.apple.com/wwdc21/10132>

## Async Function

```swift
func fetchThumbnail(for id:String) async throws -> UIImage {
    let request = thumbnailURLRequest(for: id)
    let (data, response) = try await URLSession.shared.data(for: request)
    guard (response as? HTTPURLResponse)?.statusCode == 200 else { throw FetchError.badID }
    let maybeImage = UIImage(data: data)
    guard let thumbnail = await maybeImage?.thumbnail else { throw FetchError.badImage }
    return thumbnail
}
```

## Async property

```swift
extension UIImage {
    var thumbnail: UIImage? {
        get async {
            let size = CGSize(width: 40, height: 40)
            return await self.byPreparingThumbnail(ofSize: size)
        }
    }
}
```

## Async sequences

```swift
for await in staticImageIDsURL.lines {
    let thumbnail = await fetchThumbnail(for: id)
    collage.add(thumbnail)
}
let result = await collage.draw()
```

`await`は`for`ループでも動作する！

## Async APIs in the SDK

```swift
URLSession.data(with: URLRequest, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void)
```

↓

```swift
URLSession.data(with: URLRequest) async throws -> (Data, URLResponse)
```

## Checked continueations

- 継続は全ての経路において確実に一度だけ再開される
- 再開せずに継続を破棄することは許されない
- Swiftはあなたのコードをチェックする

```swift
func persistentPosts() async throws -> [Post] {
    return try await withCheckedThrowingContinuation { continuation in
        self.getPersistentPosts { posts, error in
            if let error = error {
                continuation.resume(throwing: error)
            } else {
                continuation.resume(returning: data)
                continuation.resume(returning: data) // ❌
            }
        }
    }
}
```
