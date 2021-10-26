# ElastiCache Service Update

## [メンテナンスのヘルプ - Amazon ElastiCache | AWS](https://aws.amazon.com/jp/elasticache/elasticache-maintenance/?nc1=h_ls)

> サービスアップデートは、お客様ご自身で柔軟に適用することができます。サービスアップデートには時間が設定されており、期限が切れた後にメンテナンスウィンドウに移動して当社が適用することができます。


### サービスアップデート

#### Q：メンテナンスウィンドウで適用されるアップデートは、サービスアップデートとどう違うか？

アップデートの種類。

- 継続的管理型メンテナンス

  メンテナンスウィンドウに直接スケジュールされ、お客様側での操作は不要。
  ❓操作不可能ってこと？

- サービスアップデート

  時間指定で、"推奨される適用日(`Recommended Apply by Date`)"によって適用するタイミングをコントロールすることができます。
  ❓いや、これが設定できないんだけど。
 
#### Q: ElastiCache for Redis クラスタにサービスアップデートを適用した場合、どのような影響がありますか？データやクラスターへの接続性が失われますか？

いいえ、Redisクラスターは引き続きトラフィックを提供します。**数秒のダウンタイムが発生します**。

サービスアップデートを適用する1つまたは複数のRedisクラスターを選択すると、Amazon ElastiCacheは、選択されたすべてのクラスターがアップデートされるまで、すべてのシャードと並行して1ノードずつアップデートを適用します。

#### Q: サービスアップデートを停止することはできますか？

はい。

しかし、サービスアップデートの `Auto-Update after Due Date`属性の値が `yes`で、`Recommended Apply by Date`が経過している場合、ElastiCacheは次のメンテナンスウィンドウで残りのクラスターにサービスアップデートをスケジュールします。

❓この記載通りにならなかった。

それでも、メンテナンスウィンドウの前にサービスアップデートを残りのクラスターに適用した場合、ElastiCacheはメンテナンスウィンドウの間にサービスアップデートを再適用しません。

#### Q: サービスアップデートをメンテナンスウィンドウ中にElastiCacheで直接適用することができないのはなぜですか？

サービスアップデートの目的は、アップデートを適用するタイミングを柔軟に設定することにあります。

ElastiCacheがサポートしているコンプライアンスプログラムに参加していないクラスターは、これらのアップデートを適用しないか、あるいは年間を通して頻度を落として適用することができます。

これは、サービスアップデートの`Auto-Update after Due Date`属性の値が`no`の場合に限ります。

#### Q: 期限切れのサービス更新を適用するノードがある場合はどうなりますか? 次のサービス更新を待つ必要がありますか?

新しいノードには、該当するすべてのサービス更新が含まれているため、更新されていない既存のノードを手動で置き換えて、最新の更新を入手できます。

### 継続的な管理メンテナンスの更新

#### Q: 継続的な管理メンテナンスの更新とは何ですか?

これらの更新は必須であり、メンテナンスウィンドウ内で直接適用されます。あなたのアクションは不要です。

これらの更新は、**サービス更新によって提供される更新とは別のもの**です。

## [Applying the self-service updates - Amazon ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/applying-updates.html#applying-updates-console-redis-console)

> **Auto-Update after Due Date**: If this attribute is yes, after the Recommended apply by Date has passed, ElastiCache schedules clusters yet to be updated in the appropriate maintenance window. Updates are applied along with other applicable updates. You can continue to apply the updates until the update expiration date.

`Auto-Update after Due Date` がtrueで `Recommended apply by Date` を経過した場合、サービスアップデートがメンテナンスウィンドウ内で実行されるように計画される。
