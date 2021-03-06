# SSL Pinning

中間者攻撃(Man in the Middle)に対応する手段。

2種類ある。

## Certificate Pinning

最も簡単な方法。
証明書をクライアントプログラムに埋め込む（腹持ち）します。

クライアントは実行時に、コールバックでサーバの証明書を取得します。
そのコールバックの中で、取得した証明書とプログラムに埋め込まれた証明書を比較します。
比較に失敗した場合は、メソッド(や関数)を失敗させます。

この方法はデメリットがあり、サーバーの証明書のローテーションに合わせて、アプリケーションを更新する必要があります。
（埋め込む証明書を更新すると言う事）

## Public Key Pinning

公開鍵のピン留めはより柔軟な方法ですが、証明書から公開鍵を抽出するために余分な手間があります。
証明書ピン留めと同様に、クライアントは抽出された公開鍵を、埋め込まれた公開鍵のコピーで確認します。
公開鍵をクライアントに埋め込みます。

公開鍵のピン留めには2つの欠点があります。
第一に、通常は証明書から鍵を抽出しなければならないので、鍵(対証明書)の取り扱いに一手間増えます。
第二に、鍵は静的なものであり、鍵のローテーションポリシーに違反する可能性があります。

上記は Pre-loaded Public Key Pinning と言う手法。

これとは別に HTTP based Public Key Pinningと言う方法がある。

参考:
[HTTP Public Key Pinning (HPKP) - Web セキュリティ | MDN](https://developer.mozilla.org/ja/docs/Web/Security/Public_Key_Pinning)
