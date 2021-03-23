# Amazon EMR - Work with Storage And File System

via [Work with Storage and File Systems - Amazon EMR](https://docs.aws.amazon.com/ja_jp/emr/latest/ManagementGuide/emr-plan-file-systems.html)

どのファイルシステムを使用するかは、データへのアクセスに使用する URI のプレフィックスで指定する。

## HDFS

`hdfs://` (もしくはプレフィックスなし)

Hadoopファイルシステム。
エフェメラルストレージで高速である。ジョブフローの中間ステップで得られた結果のキャッシュに最適。

## EMRFS

`s3://`

通常のファイルをEMRからS3に直接読み書きするために使用されるHadoopファイルシステム実装。

## ローカルファイルシステム

Hadoopクラスタのノードはインスタンスストアなec2であり、そのファイルシステムを使用できる。

## S3ブロックファイルシステム

`s3bfs://`

レガシーシステムなので使用非推奨。
