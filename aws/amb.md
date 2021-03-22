# Amazon Managed Blockchain (AMB)

via [20200519 AWS Black Belt Online Seminar Amazon Managed Blockchain (AMB)](https://www.slideshare.net/AmazonWebServicesJapan/20200519-aws-black-belt-online-seminar-amazon-managed-blockchain-amb)

## ブロックチェーンとは

トランザクションが、安全である事を検証され、保証するために信頼できる中央機関を必要とせずに、複数の当事者がトランザクションを台帳に記録できる事を可能にする技術。

## ブロックチェーンの三つの構成要素

1. Ledgers (台帳)
    - イミュータブル
    - 追加のみ
    - 暗号的に検証
2. Decentralization (非中央集権)
    - 信頼されたデータの分散同期
        - ビサンチン・フォールト・トレランス(PBFT)
        - Proof of Work (PoW)
        - ...など
3. Consensus algorithms (コンセンサスアルゴリズム)
    - スマートコントラクトのための仲介者を排除した合意形成プロセス

## ブロックチェーンの種類

- パブリックネットワーク
    - ビットコインなど
- 許可型 (Permissioned) ネットワーク
    - Hyperledger Fabricなど

## AWSによるブロックチェーンサービス

1. 信頼された中央機関による台帳
2. 信頼された分散期間によるトランザクション

### AWSが提供するマネージドサービス

- Amazon Quantum Ledger Database (QLDB)
    - イミュータブル
    - 暗号的に検証可能
    - スケーラブル
    - 安易な操作性
- Amazon Managed Blockchain (AMB)
    - フルマネージド
    - Hyperledger FabricとEtherreumを選択可能
    - スケーラブルかつセキュア
    - 信頼性の向上

#### Hyperledger Fabric

許可型ネットワーク。

厳格なプライバシーおよび権限管理が必要なアプリケーションの場合。例えば、特定の取引関連データが特定の銀行とのみ共有され、ネットワーク内の他のメンバーがそのデータにアクセスできない場合など。

##### 主要コンポーネント

- Ordering Service

  トランザクションの配信と順序づけを行う。

  オープンソースの場合はApach Kafkaを利用する。

- Certificate Authority

  Hyperledger Fabric用のCAが用意されており、証明書の発行は失効の管理を行う。

  オープンソースの場合は「ソフト」HSMを使用する。

  AWS KMSを使用して認証局サービスを保護する。

- Peer Node

  実際の台帳データベースのコピーを保存する。

#### Ethereum

公開ネットワーク。

全てのメンバーのデータの透過性が重要なアプリケーションの場合。例えば、農家と政府機関で構成されるネットワークでは、無数の農家がネットワークに参加し、土地や作物の収穫量などに関する情報がブロックチェーン上の全てのメンバーで共有されます。

## ブロックチェーンの課題

- 設定が複雑
- スケールするのが難しい
- 管理が複雑
- コスト

## Amazon Managed Blockchainの構築フロー

1. ネットワークを作成する

   ブロックチェーン・フレームワークを選択し、クリックするだけで、ブロックチェーン・ネットワークとAWSアカウントのメンバーしっぷを設定することができる。

2. メンバーを招待する

   他のAWSアカウントを招待してネットワークに参加することができる。

3. ノードを追加する

   分散台帳のコピーを格納するブロックチェーンピアノーどを作成および構成する。

### 誰がブロックチェーンネットワークを所有するのか？

ネットワークは分散されており、最初の作成者が去った後でもアクティブなままでいることができる。

メンバーは、メンバーの招待と削除、ネットワークルールの構成に投票する。

各メンバーはリソースに支払う。

## 参考

- [【AWS Black Belt Online Seminar】Amazon Managed Blockchain (AMB) - YouTube](https://www.youtube.com/watch?v=GvTvJIbxWdw)
- 2020/03/26 東京リージョン対応
