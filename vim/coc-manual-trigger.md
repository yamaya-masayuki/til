# cocで補完を手動で起動する

[coc.nvim](https://github.com/neoclide/coc.nvim)の補完ポップアップメニューはデフォルトで自動的に表示される。
特定のキーマップでポップアップメニューを表示する設定は以下の通り。

- `coc-settings.json`で`suggest.autoTrigger`を`none`にする
- キーマップする: `inoremap <expr> <C-Space> coc#refresh()`

これで CTRL-Space で補完ポップアップメニューが表示される。
