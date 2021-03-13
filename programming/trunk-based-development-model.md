# trunk ベース開発モデル

- [DevOps 技術: トランクベース開発 | Google Cloud](https://cloud.google.com/solutions/devops/devops-tech-trunk-based-development/?hl=ja)
- [Trunk Based Development](https://trunkbaseddevelopment.com/)

このモデルの狙いは **大規模なマージ作業を発生させないようにする**。

- 1つの `trunk` ブランチと、複数の `release` ブランチだけ
    - `feature`とか`epic`ブランチは存在しない
- コミット先は `trunk`
- `release`はリリース時に生やす
    - バグがなければ追加のcommitは無い
- ホットフィックスは例外で`release`から `trunk` へのマージする
    - 他の `release` はcherry pickする

上記を実現するために`trunk`へのマージ単位は小粒で。

予想される悩みどころ:

- リリース範囲外の機能や修正もリリースされる可能性が高まる
    - ここでケーパビリティ機能(実行時の機能ON/OFF)の話しが出てくるのだろう。
- 実装作業ボリュームをマージする単位で考える必要がある（しかもできるだけ小さい単位で）
    - 機能単位から非機能単位をブレークダウンする必要あり

機能保証をどの局面で誰が行うのかを決める必要ありそう。
もしくはリリース隊長を用意するか。
