# sips: 画像ファイルの形式を変換する

macOS標準コマンド。
3rdパーティ製のものをインストールしなくても良い。

例: JPEGからPNGへの変換。

```bash
sips -s format png in.jpeg --out out.png
```

対応している画像形式は

- jpeg
- tiff
- png
- gif
- jp2
- pict
- bmp
- qtif
- psd
- sgi
- tga

そもそもコマンドは、ラスター画像ファイルやColorSync ICCプロファイルを照会・修正するためのもので、AppleScript Suite の`Image Events` の実装になっている。
また、画像を修正・生成するためのJavaScriptの実行も可能。
