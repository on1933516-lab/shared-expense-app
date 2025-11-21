 architecture.md

１．	利用者がCloudFront の URL にアクセスする。
　　　 この時、WAFで許可/拒否を判定する。
     （１）	許可
            ⇒２．へ進む。
     （２）	拒否
            ⇒CloudFrontのerror response 設定によりsorryページを表示する。

２．	CloudFront はオリジンである S3 にリクエストする。
      デフォルトルートオブジェクト設定に従い、
      S3 から index.html を取得して利用者へ返す。
     （flutterベースでUI等フロント部分やAPI呼び出しを構築）

３．	API Gateway がリクエストを受け取る。
      Flutter（ブラウザ）でのユーザー操作によって、
      API 呼び出し（POST / GET / PUT / DELETE）が API Gateway に送られる。
      API Gateway は HTTP API として動作しており、適切な Lambda を呼び出す。
     （１）/expenses/POST
　　　     Lambda：shared-expenses-create
　　　     処理内容：DynamoDB に新規データを登録
     （２）/expenses/GET
　　     　Lambda：shared-expenses-get
　     　　処理内容：DynamoDB からデータ一覧を取得し、日付順で返す
     （３）/expenses/{id}/DELETE
　     　　Lambda：shared-expenses-delete
　　　     処理内容：指定 id のデータを DynamoDB から削除
     （４）/expenses/{id}/PUT
　　     　Lambda：shared-expenses-update
           処理内容：指定 id の項目の settled を true（精算済み） に更新

４．DynamoDBのテーブル構成
　　
　　　| カラム名 | 説明 | 型 |
　　　|---------|------|-----|
　　　| id      | 一意のID（PK） | String |
　　　| amount  | 金額 | Number |
　　　| category | 項目名 | String |
　　　| date    | 日付（YYYY-MM-DD） | String |
　　　| members | 人数 | Number |
　　　| note    | 備考 | String |
　　　| payer   | 支払者名 | String |
　　　| settled | 精算済みかど



 
