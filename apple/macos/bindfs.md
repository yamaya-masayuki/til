# macOS Big Sur以降でbindfsを使う

## 基本

homebrewのbindfsは無効になっている。
代替手段として`gromgit/fuse`を使う。

インストール済みのFUSEファイルシステムがあるならば、アンインストールしておくこと。

```bash
brew install macfuse
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

## Apple Silicon では

- 起動セキュリティを**低セキュリティ**にする
- **確認済みの開発元から提供されたカーネル拡張のユーザ管理を許可**を選択にする
