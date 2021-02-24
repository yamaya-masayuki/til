# Packerでエラー時にec2インスタンスにsshする

デバッグモードで起動すると、ステップ実行になる。
エラーになったステップで止めておいて、ワーキングディレクトリに置かれている秘密鍵(pem)ファイルを使ってsshする。

```bash
packer build --debug target.json
```

別のshellから、

```bash
ssh -i ./ec2_amazon-ebs.pem ec2-user@{IPアドレス}
```

とする。

packerコマンドを終了させると、クリーンナップによりインスタンスもpemも削除される。
それが嫌な場合は `-on-error` オプションを使うべし。
