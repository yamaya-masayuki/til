# TAPI (Terminal API)

terminalで実行しているshellからvimにコマンドを送信することができる。

## 形式

`<ESC>]51;msg<07>`

- `<ESC>` は `0x5c`
- `<07>` は `0x07`
- `msg` にはJSONの配列を埋め込む
    - 先頭要素にはコマンド
        - `drop`
            - ファイルを開く(`:e` 相当)
        - `call`
            - 関数呼び出し
    - 第二要素以降はコマンドの引数

## 例

```shell
echo -e "\x1b]51;[\"drop\",\"/u/foo/.vimrc\"
```

このコマンドラインを実行すると、`/u/foo/.vimrc`バッファが開かれる。

## ヘルプ

`:help terminal-api` を見よう。
