# sbt testOnlyでプロジェクトも絞り込む

`sbt` のコマンドライン引数全体をクォートで囲む

```shell
sbt "testOnly my.package.TheClassTest"
```

参考: <https://stackoverflow.com/a/57729537/8064892>
