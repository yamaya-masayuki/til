# VPN CloudHub

複数の Site-to-Site VPN 接続がある場合は、AWS VPN CloudHub を使用して、安全なサイト間通信を提供することができます。

VPC の有無にかかわらず使用できるシンプルな"Hub and Spork"モデルで動作します。

![Architecture](https://docs.aws.amazon.com/ja_jp/vpn/latest/s2svpn/images/AWS_VPN_CloudHub-diagram.png)

1. 仮想プライベートゲートウェイは1つ
2. カスタマーゲートウェイは顧客のネットワーク事に作成する
    - パブリックIPも持つ
    - カスターゲートウェイのBGP自律システム番号(ASN)が必要
3. 各カスタマーゲートウェイから仮想プライベートゲートウェイに動的ルーティングされる Site-to-Site VPN 接続を作成する
4. 仮想プライベートゲートウェイに、サイト固有のプレフィックス(10.0.0.0/24など)をアドバタイズするように、カスタマーゲートウェイを設定する
5. サブネットルートテーブルのルートを設定して、VPC内のインスタンスがサイト(って何？)と通信できるように設定します

