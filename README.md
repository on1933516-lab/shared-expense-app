# shared-expense-app
生活費管理システム

### 1. 概要

- **システム名**：共同生活費管理アプリ
- **目的**：2人間の共通支出を登録・集計・精算管理する
- **想定ユーザー**：開発者本人＋同棲相手（2名）
- **アクセス方法**：スマホブラウザ（S3ホスティング）

### 2. 全体構成図（例）

```
スマホブラウザ
　↓（HTTPS）
[S3 静的Webサイト]
　↓（fetch API）
[API Gateway]
　↓
[Lambda関数]
　↓
[DynamoDB]

```

※ CloudWatchでログ監視。

※ APIキーによる簡易認証。

### 3. 使用サービス一覧

| 区分 | サービス | 用途 | 備考 |
| --- | --- | --- | --- |
| フロント | S3 + CloudFront | 静的サイトホスティング | HTML/JSでAPI呼び出し |
| API | API Gateway | リクエスト受け口 | CORS設定要 |
| アプリ | Lambda (Python or Node.js) | 業務ロジック | CRUD処理を担当 |
| DB | DynamoDB | データ保存 | 立替情報を格納 |
| 監視 | CloudWatch | ログ収集 | エラー検知 |
| 認証 | API Key | 共通利用制限 | Cognitoは未使用 |

### 4. データ設計（DynamoDB）

| 項目名 | 型 | 説明 |
| --- | --- | --- |
| id | string | UUID |
| category | string | 家賃・水道代など |
| payer | string | 立替者（you / partner） |
| amount | number | 金額 |
| date | string | 支払日（YYYY-MM-DD） |
| settled | boolean | 精算済みフラグ |
| note | string (optional) | 備考 |

### 5. API設計

| メソッド | パス | 機能 | 入力例 | 備考 |
| --- | --- | --- | --- | --- |
| POST | /expenses | 支出登録 | JSON本文 | Lambda→PutItem |
| GET | /expenses | 支出一覧取得 | - | settled=false で絞込 |
| PUT | /expenses/{id} | 精算更新 | id, settled=true |  |
| DELETE | /expenses/{id} | 削除 | id | 任意 |

### 6. セキュリティ・アクセス制御

- **認証方式**：APIキー（共通）
- **CORS設定**：S3のドメインのみ許可
- **IAMロール**：LambdaにDynamoDB操作権限付与
- **データ保護**：HTTPS通信／DynamoDB暗号化デフォルトON

### 7. 運用・保守メモ

- **デプロイ**：AWS CLI or コンソール操作
- **ログ確認**：CloudWatch Logs
- **課金対策**：DynamoDBオンデマンド／月1回コスト確認
- **バックアップ**：DynamoDBオンデマンドバックアップ

### 8. 今後の拡張アイデア

- ユーザーごと認証（Cognito連携）
- 支出カテゴリー別グラフ表示
- LINE通知で精算リマインド
