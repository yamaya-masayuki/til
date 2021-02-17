# Dev Mark - サイトにdevelopマークやproductionマークを付けるChrome拡張

[Dev Mark - Chrome ウェブストア](https://chrome.google.com/webstore/detail/dev-mark/bnpplihcampoabjlbcchijpcinnjdbjp)

## 設定方法

### グローバルな設定

Dev Mark拡張をクリックして `オプション` メニューを選択。
jsonを入力するページが表示される。

```json
{
  "configs": [
    {
      "env": "Staging",
      "regexp": "(staging|-stg-)",
      "color": "blue"
    }
  ]
}
```

`env` はマークに表示される文字列。
`regex` はURLに対する正規表現。これにマッチするとマークが表示される。
`color` はマークの背景色。選択可能な色はページ上部に表示されている。

### サイト毎の設定

1. 対象のサイトを開く
2. Dev Mark拡張をクリックして `サイトデータの読み取りと変更を行います` メニューを選択

