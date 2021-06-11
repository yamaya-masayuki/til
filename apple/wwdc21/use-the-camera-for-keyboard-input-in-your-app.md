# Use the camera for keyboard input in your app

<https://developer.apple.com/wwdc21/10276>

## カメラからのテキスト入力

テキストフィールドの編集メニューに `Text from Camera` が追加され、それを選択すると、カメラが起動し、撮影した写真でテキスト認識を行う。ユーザーはテキストをコピペすれば良い。

古いハードウェアだと動かない。

## テキストコンテンツフィルタ

追加されるタイプ

- `.FlightNumber` ... フライト番号
- `.ShipmentTrackingNumber` ... 荷物追跡番号
- `.DateTime` ... 日時

