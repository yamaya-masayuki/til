# Packerでエラー時にインスタンスを終了しない方法

`build` サブコマンドの`-on-error`オプションを指定する。

```sh
packer build -on-error=ask
```

指定できる値。

- `cleanup`
  デフォルト。インスタンスを終了する。
- `abort`
  インスタンスをキープする。
- `ask`
  インスタンスを終了するかキープするか選択できる。
- `run-cleanup-provisioner`
  `error-cleanup-provisioner` が定義されている場合は `error-cleanup-provisioner` 以外のクリーンアップを行わずにabort。

参照: [packer build - Commands | Packer by HashiCorp](https://www.packer.io/docs/commands/build)
