# 再起動がともなうEC2のスケジュールイベントのリスケ

[再起動がともなうAmazon EC2のスケジュールイベントの開始時間を変更出来る機能がリリースされました。 | DevelopersIO](https://dev.classmethod.jp/articles/reschedule-amazon-ec2-scheduled-events/)

## 条件や制約

- Event Deadline を持つ再起動イベントのみ有効
- Event Deadline より前の日時で設定が可能
- 未実行のイベントのみが対象
- 5分以内に開始が予定されているイベントについては変更できない
- 新しいイベント開始時刻は、現在時刻から**60分以上後**である必要がある

