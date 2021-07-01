# macOS Big Surでbindfsを使う

homebrewのbindfsは無効になっている。
代替手段として`gromgit/fuse`を使う。

インストール済みのFUSEファイルシステムがあるならば、アンインストールしておくこと。

```bash
brew install --cask macfuse
brew install gromgit/fuse/bindfs-mac
```

マウントは以下のようにやる。

```bash
mkdir ~/.local/mnt
bindfs '~/Library/Mobile Documents/com~apple~CloudDocs' ~/.local/mnt
```

マウントしているかどうかは以下のように判定できる。

```fish
# fish shell

mount | grep bindfs > /dev/null
if test $status -eq 0
  echo 'hi! bindfs mount point exists.'
end
```
