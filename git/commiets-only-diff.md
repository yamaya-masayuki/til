# ブランチ間diffでコミットのタイトルだけ表示する方法

`log` サブコマンドを使います。

```sh
git log main..develop --pretty='format:%cs %s'
```

Output

```text
2021-01-28 Merge pull request #77 from fix/datadog-apm-disabled
2021-01-25 動作確認時に発見した不備の修正
```
