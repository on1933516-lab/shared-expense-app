# Shared Expense App（生活費管理アプリ）

家族やペア向けの **割り勘・精算管理アプリ** です。  
Flutter Web + AWS（CloudFront / WAF / API Gateway / Lambda / DynamoDB / S3）で構築しています。

---

## 主な機能

- 支出の登録（カテゴリ・金額・日付・支払者など）
- 支出一覧（未精算 / 精算済み 切替）
- 精算処理（PUT）
- 削除（DELETE）
- DynamoDB でデータ永続化
- CloudFront + WAF でアクセス制御
- CloudWatch ログ

---

## 使用技術

### フロントエンド
- Flutter Web
- Dart

### バックエンド（API）
- AWS API Gateway（HTTP API）
- AWS Lambda  
  - `shared-expenses-create`（POST）
  - `shared-expenses-get`（GET）
  - `shared-expenses-delete`（DELETE）
  - `shared-expenses-update`（PUT）

### インフラ
- AWS CloudFront（CDN）
- AWS WAF（IP 制御）
- AWS S3（フロントホスティング）
- AWS DynamoDB（NoSQL DB）
- AWS CloudWatch（ログ監視）

---



