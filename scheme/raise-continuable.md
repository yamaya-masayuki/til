# Schemeには例外の再開という機能がある

大域脱出ではなく、raise地点に戻ることができる。

```scheme
; 例外ハンドラの登録
(with-exception-handler handler thunk)

; 再開可能な例外を送出する
(raise-continuable obj)
```

## with-exception-handler

thunk内で例外が発生するとhandlerが実行される。
thunkは無引数な手続き。

## raise-continuable

例外を発生させる。objは例外ハンドラの引数に渡される。
例外ハンドラの実行が終わると戻ってくる。
返り値は例外ハンドラの結果。
