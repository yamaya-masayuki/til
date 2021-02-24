# Packerでエラー時にec2インスタンスにsshする

デバッグモードで起動すると、ステップ実行になる。
エラーになったステップで止めておいて、ワーキングディレクトリに置かれている秘密鍵(pem)ファイルを使ってsshする。

```bash
packer build --debug target.json
:
==> amazon-ebs: Waiting for instance (i-00000000000000000) to become ready...
    amazon-ebs: Public DNS: ec2-00-000-00-000.ap-northeast-1.compute.amazonaws.com
    amazon-ebs: Public IP: 00.000.00.000
    amazon-ebs: Private IP: 00.000.00.000
```

上記の `Public IP:` に出力されているIPアドレスを使ってsshする。

```bash
ssh -i ./ec2_amazon-ebs.pem ec2-user@{IPアドレス}
```

packerコマンドを終了させると、クリーンナップによりインスタンスもpemも削除される。
それが嫌な場合は `-on-error` オプションを使うべし。
