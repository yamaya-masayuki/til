# Detect people, faces, and poses using Vision

<https://developer.apple.com/wwdc21/10040>

## 顔認識

## 人分割 (Person Segmentation)

風景から人を分割することを可能にする。

ユースケース:

- 仮想背景、スポーツ分析、自動運転など
- ポートレイトモード

```swift
// Create request 
let request = VNGeneratePersonSegmentationRequest()

// Create request handler
let requestHandler = VNImageRequestHandler(url: imageURL, options: options)

// Process request
try requestHandler.perform([request])

// Review results
let mask = request.results!.first!
let maskBuffer = mask.pixelBuffer
```

### マスクの適用

風景から人を除いたマスクを作り、人と別の風景を合成する。

```swift
let input = CIImage?(contentsOf: imageUrl)!
let mask = CIImage(cvPixelBuffer: maskBuffer)
let background = CIImage?(contentsOf: backgroundImageUrl)!

let maskScaleX = input.extent.width / mask.extent.width
let maskScaleY = input.extent.height / mask.extent.height
let maskScaled = mask.transformed(by: __CGAffineTransformMake(
                                  maskScaleX, 0, 0, maskScaleY, 0, 0))

let backgroundScaleX = input.extent.width / background.extent.width
let backgroundScaleY = input.extent.height / background.extent.height
let backgroundScaled = background.transformed(by: __CGAffineTransformMake(
                          backgroundScaleX, 0, 0, backgroundScaleY, 0, 0))

let blendFilter = CIFilter.blendWithRedMask()
blendFilter.inputImage = input
blendFilter.backgroundImage = backgroundScaled 
blendFilter.maskImage = maskScaled

let blendedImage = blendFilter.outputImage
```

