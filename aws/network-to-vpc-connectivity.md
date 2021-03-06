# Network-to-Amazon VPC connectivity options

[Network-to-Amazon VPC connectivity options - Amazon Virtual Private Cloud Connectivity Options](https://docs.aws.amazon.com/ja_jp/whitepapers/latest/aws-vpc-connectivity-options/network-to-amazon-vpc-connectivity-options.html)の勝手訳。

このセクションでは、リモートネットワークをAmazon VPC環境に接続するためのデザインパターンを提供します。これらのオプションは、内部ネットワークをAWSクラウドに拡張することで、AWSリソースを既存のオンサイトサービス（監視、認証、セキュリティ、データ、その他のシステムなど）と統合するのに便利です。また、このネットワーク拡張により、内部ユーザーは、他の社内向けリソースと同様に、AWS上でホストされているリソースにシームレスに接続することができます。

リモートの顧客ネットワークへのVPC接続は、接続する各ネットワークに重複しないIPレンジを使用することで最適になります。例えば、1つ、または、複数のVPCをホームネットワークに接続する場合、それぞれのVPCに固有のCIDR範囲が設定されていることを確認してください。各VPCで使用するために、1つの連続した重複しないCIDRブロックを割り当てることをお勧めします。

Amazon VPCのルーティングと制約についての追加情報は、[Amazon VPC Frequently Asked Questions](http://aws.amazon.com/vpc/faqs/)を参照してください。

## <a name="aws-managed-vpn"></a>AWSマネージドVPN

### ユースケース

AWSマネージドIPsec VPNによるインターネット経由での個別VPCへの接続。

### 利点

- 既存のVPN装置とプロセスを再利用できる。
- 既存のインターネット接続を再利用できる。
- AWSマネージドで高可用なVPNをサービスする。
- 静的ルーティング、または動的BGP(Border Gateway Protocol)ピアリング、ルーティングポリシーをサポートする。

### 欠点

- ネットワークの遅延、変動性、可用性は、インターネットの状況に依存する。
- お客様が管理するエンドポイントは、（必要に応じて）冗長性とフェイルオーバーを実装する責任があある。
- お客様の機器がシングルホップBGPをサポートしていること（BGPを動的ルーティングに使用する場合）。

## AWS Transit Gateway + VPN

### ユースケース

複数のVPCのために、インターネットを介して地域のルーターにAWSマネージドIPsec VPN接続を行う。

### 利点

- [AWSマネージドVPN](#aws-managed-vpn)と同じ。
- AWSマネージドな高可用性と最大5,000の取り付けに対応する高可用性とスケーラビリティを備えた地域ネットワークハブ。

### 欠点

- [AWSマネージドVPN](#aws-managed-vpn)と同じ。

## AWS Direct Connect

### ユースケース

専用線によるネットワーク接続。

### 利点

- 予測可能なネットワークパフォーマンスの向上。
- 帯域コストの削減。
- BGPのピアリングやルーティングポリシーをサポート。

### 欠点

- 通信事業者やホスティング事業者との関係強化や、新たなネットワーク回路の構築が必要になる可能性がある。

## AWS Direct Connect + AWS Transit Gateway

### ユースケース

複数のVPCに対応した地域ルーターへの専用線によるネットワーク接続。

### 利点

- [AWSマネージドVPN](#aws-managed-vpn)と同じ。
- AWSマネージドな高可用性と最大5,000の取り付けに対応する高可用性とスケーラビリティを備えた地域ネットワークハブ。

### 欠点

- [AWSマネージドVPN](#aws-managed-vpn)と同じ。

## AWS Direct Connect + VPN

### ユースケース

専用線によるIPsec VPN接続。

### 利点

- 予測可能なネットワークパフォーマンスの向上。
- 帯域幅コストの削減。
- AWS Direct ConnectのBGPピアリングとルーティングポリシーをサポート。
- 既存のVPN機器やプロセスの再利用。
- AWSマネージドで高可用なVPNサービス。
- VPN接続における静的ルーティングまたは動的BGP(Border Gateway Protocol)ピアリングおよびルーティングポリシーをサポート。

### 欠点

- 通信事業者やホスティング事業者との関係強化や、新たなネットワーク回路の構築が必要となる場合がある。
- お客様が管理するエンドポイントは、（必要に応じて）冗長性とフェイルオーバーを実装する責任がある。
- お客様の装置は、シングルホップBGPをサポートする必要がある（BGPダイナミックルーティングを使用する場合）。

## AWS Direct Connect + AWS Transit Gateway + VPN

### ユースケース

複数のVPCに対応した地域ルータへの専用線によるIPSec VPN接続。

### 利点

- [AWSマネージドVPN](#aws-managed-vpn)と同じ。
- AWSマネージドな高可用性と最大5,000の取り付けに対応する高可用性とスケーラビリティを備えた地域ネットワークハブ。

### 欠点

- [AWSマネージドVPN](#aws-managed-vpn)と同じ。

## AWS VPN CloudHub

### ユースケース

リモートの支店をハブ＆スポークモデルで接続し、プライマリまたはバックアップ接続を実現する。

### 利点

- 既存のインターネット接続とAWSのVPN接続を再利用できる。
- AWSマネージドで高可用なVPNサービス。
- BGP経路交換とルーティングポリシーをサポートする。

### 欠点

- ネットワークの遅延、変動性、可用性はインターネットに依存します。
- ユーザーが管理する支店のエンドポイントは、冗長性とフェイルオーバー（必要な場合）を実装する責任がある。

## ソフトウェア・サイト間VPN

### ユースケース

ソフトウェア部品によるインターネット上のVPN接続。

### 利点

- より多くのVPNベンダー、製品、プロトコルをサポートする。
- 完全に顧客管理されたソリューションである。

### 欠点

- お客様は、必要に応じて、すべてのVPNエンドポイントにHA（ハイ・アベイラビリティ）ソリューションを導入する責任がある。
