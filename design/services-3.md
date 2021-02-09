# サービスには3種類ある

via

- [architecture - Domain Driven Design: Domain Service, Application Service - Stack Overflow](https://stackoverflow.com/a/2279729/8064892)
- [texta.fm - 3. Low-Code Development](https://podcasts.google.com/feed/aHR0cHM6Ly9hbmNob3IuZm0vcy8zMzBhOTQ4OC9wb2RjYXN0L3Jzcw/episode/MDE4NDk3YzQtNDgzMC00ZGRmLWE1YjktZGY3MjdhNjA5NzA2?sa=X&ved=0CA0QkfYCahcKEwjAloyawdzuAhUAAAAAHQAAAAAQAQ)

- ドメインサービス

  ドメインオブジェクト内に収まらないビジネスロジックをカプセル化したもの。
  いわゆるCRUD操作操作ではありません。
  Domain Entityにも Value Objectにも配置出来ないロジックを配置する場所をさす。Service Objectとも。

- アプリケーションサービス

  外部のコンシューマーがシステムと対話するために使用します（Webサービスを考えてみてくだ
  さい）。コンシューマがCRUD操作にアクセスする必要がある場合は、ここで公開され
  ます。
  ユースケースに近い。
  トランザクションスクリプトの置き場所。

- インフラストラクチャサービス

  技術的な問題を抽象化するために使用されます。
  (例: MSMQ、電子メールプロバイダなど)。
  分散システムにおけるノードが請け負うロール・機能群。

ドメインサービスをドメインオブジェクトと一緒に維持するのは賢明です。
また、リポジトリを注入することもできます。

アプリケーションサービスは通常、ドメインサービスとリポジトリの両方を使用して外
部からのリクエストを処理します。
