# AWS Summit 2023

## ニコニコでのクラウド活用、その意思決定とアーキテクチャ

- ニコニコの開発は大規模オンプレミス
  - 80 を超えるシステム数、相互依存関係あり
  - 長年の蓄積もあり整備されている
  - 社内マネージドサービスでは賄いきれないものを自社で作るのは大変なのでクラウドを活用していきたい
- 地方分権とガバナンス
  - 開発チームが自立してクラウドサービスを使用できる
- アカウント管理
  - Control Tower
  - IAM Identity Center
- ネットワーク
  - VPC IP Address Manager (IPAM)
    - IP アドレスの管理を自動化する
  - Direct Connect + Transit Gateway
  - Lambda で Transit Gateway の承認を自動化
- 名前解決
  - Route 53 Resolver Endpoint
  - Route 53 Private Hosted Zone
- 推進活動
  - AWS 勉強会の実施
  - AWS Office Hour の活用

## AWS Connect とデータ分析で実現するコンタクトセンターの改善

- Amazon Connect
- Contact Lenz for Amazon Connect
  - 通話内容を自動文字起こし
- Amazon Pinpoint
  - Email, SMS, プッシュ通知の送信

## Everything fails, all the time: 分散システムにおける耐障害性のある設計について

- 旅の予約アプリを例にする
- 何故マイクロサービスにしたいか
  - 飛行機の予約機能だけスケールアウトできるようにしたい
  - 支払機能だけ追加機能を入れたい
- ネットワークが挟むと障害の起こるリスクが上がる
  - モノリスでもクライアントとサーバでネットワークを挟むがコンポーネント間はネットワークを挟まない
  - マイクロサービスはネットワークを挟むことが多い
- 一時的なエラーでリクエストが失敗する
  - 自動リトライ
  - エクスポネンシャルバックオフでリトライ間隔を指数関数的に空けることでリトライによる負荷を減らす
  - ジッターでリトライの間隔をランダムに変える
  - エクスポネンシャルバックオフとジッターを組み合わせて使用する
  - AWS SDK には実装済み
  - エクスポネンシャルバックオフは Step Functions で実装できる
- 原因不明な問題によるタイムアウト
  - 同期実行だと待ちが発生
  - サーキットブレーカーパターンで問題の起きているサービスを一時的に停止する
  - サーキットブレーカーパターンはライブラリを使用する、サービスメッシュ(AWS App Mesh)、Step FUnctions で実装できる
- 分散されたデータの一貫性を担保する
  - ポリグロット・バーシステンスでサービスごとに DB を選択できるように設計する
  - Saga パターンによる分散トランザクションで処理が失敗した場合に、一連の流れをロールバックする
  - Saga パターンは Step Functions で実装できる
- 障害パターンを網羅的に予測できるか
  - AZ 障害が起きた場合にシステム全体として定常状態を保てるか
  - 定常状態とはビジネス的な指標として考える
  - カオスエンジニアリングで本番環境に障害を起こしてテストする
  - Fault Injection Simulator でカオスエンジニアリングを実施できる

