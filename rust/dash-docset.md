# rustクレートのdocsetを作る

[Dash](https://kapeli.com/dash)デフォルトのrust docsetでは標準ライブラリまでしか含まれない。
3rdパーティなクレートのdocsetを作るには [rust-dash-docset-gen](https://github.com/cmyr/rust-dash-docset-gen) を使う。

## インストール

README.md通りなのだが…

```shell
git clone https://github.com/cmyr/rust-dash-docset-gen.git
cd rust-dash-docset-get
python3 -mpip install --user requests
brew install dashing
cargo install rsdocs-dashing
```

## 実行

```shell
./gen_docsets.py tokio
open docsets/tokio.docset # Dash.appへインポートされる
```
