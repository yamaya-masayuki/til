# Immerse your app in spatial audio

<https://developer.apple.com/wwdc21/10265>

TBD:

## spatial audio(空間音響)とは何か？

- シアターのような体験
- 音響心理学(Psychoacoustic)的技術
- マルチ・チャンネル
- ステレオ
- オーディオビジュアルとオーディオのみ

## 空間音響の提供

- マルチチャネル・オーディオ
    - HLSとその派生種のストリームで
    - 通常のメディアファイル内のトラックで
    - WebKitによるW3C MSE経由で

## 適応性のあるメディア体験

帯域が不足して高品質な映像を提供できない場合、音声はシームレスに劣化し、アップミックスされたステレオ音声になりますが、空間的には変わりません。

移行前に提供されていたヘッドトラッキングは維持されます。

その後すぐに、帯域幅が確実に回復すると、フルマルチチャンネルの空間処理が復活します。

適応性空間オーディオでは、ステレオとマルチチャンネル間の音量レベルを正規化することがこれまで以上に重要になります。

また、DRC（Dynamic Range Control）とダイアルノルムのメタデータを適切にメディアエンコーディングで提供してください。

これについては、developer.apple.comで公開されているHLS Authoring Specificationに詳細が記載されています。

## 空間オーディオ体験を調整するAPI

`AVPlayerItem`や`AVSampleBufferAudioRenderer`を使って、アプリケーションのデフォルトの空間オーディオ体験をカスタマイズするには、4つの`AVAudioSpatializationFormats`のうちの1つを指定します。

これらは、モノラルとステレオ、マルチチャンネル、そして最後の2つの組み合わせ、つまりモノラル、ステレオ、マルチチャンネルのソースオーディオフォーマットの空間化を許可するものです。

また、0を指定すると、音声の空間化を禁止することができます。

```swift
public struct AVAudioSpatializationFormats : OptionSet {
    public init(rawValue: UInt)

    public static var monoAndStereo: AVAudioSpatializationFormats { get } 
    public static var multichannel: AVAudioSpatializationFormats { get } 
    public static var monoStereoAndMultichannel: AVAudioSpatializationFormats { get } 
}
```
