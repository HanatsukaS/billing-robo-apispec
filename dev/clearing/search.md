# 入金参照

`/api/v1.0/payment/search`

入金情報の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/payment/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                                                                                | 桁数 | 種別                               | 必須 |
| --------------------------- | ----------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                     | ユーザーID（管理画面へのログインID）                                                  | 100  | [メール形式](../../index.md#種別)  | 必須 |
| access_key                  | アクセスキー                                                                         | 100  | [半角英数](../../index.md#種別)    | 必須 |
| limit_count                 | 入金情報取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される            | 3    | 数値                               |      |
| page_count                  | 入金情報取得開始インデックス <br> ※0以上の数値を設定する。省略した場合、0が設定される   | 2    | 数値                               |      |
| [payment](#payment-request) | 入金情報に属するパラメータ                                                            |      | `array`                           |      |

#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                  | 概要                                                                                                                                                                                                                                                                                                                                                 | 桁数 | 種別         | 必須                |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ----------- | ------------------- |
| payment_id                            | 入金ID                                                                                                                                                                                                                                                                                                                                               | 18   | 数値         |                     |
| billing_name                          | 相手先名                                                                                                                                                                                                                                                                                                                                              | 100  | 文字列       |                     |
| payment_method                        | 決済方法 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP口座振替 <br> 4:RL口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段1 <br> 11:その他決済手段2 <br> 12:その他決済手段3 <br> 13:その他決済手段4 <br> 14:その他決済手段5 | 2    | 数値         |                     |
| payment_transfer_date_from            | 決済日(開始日)                                                                                                                                                                                                                                                                                                                                        | 10  | 日付         |                     |
| payment_transfer_date_to              | 決済日(終了日)                                                                                                                                                                                                                                                                                                                                        | 10  | 日付         |                     |
| payment_clearing_status               | 入金消込ステータス <br> 0:未消込み <br> 1:完了 <br> 2:確認済み                                                                                                                                                                                                                                                                                          | 1   | 数値         |                    |
| clearing_transfer_status_options      | 振替区分 <br> 0:仮受 <br> 1:入金                                                                                                                                                                                                                                                                                                                      | 1   | 数値         |                     |
| deposit_import_reserve_date_from      | 取込日(開始日)                                                                                                                                                                                                                                                                                                                                        | 10  | 日付         |                     |
| deposit_import_reserve_date_to        | 取込日(終了日)                                                                                                                                                                                                                                                                                                                                        | 10  | 日付         |                     |
| deposit_import_reserve_format_options | 取込形式 <br> 1:全銀 <br> 2:CSV-銀行振込 <br> 3:金融機関連携 <br> 5:CSV-その他 <br> 6:API                                                                                                                                                                                                                                                               | 1   | 数値         |                     |
| payment_clearing_method_options       | 消込方法 <br> 0:自動消込 <br> 1:手動消込                                                                                                                                                                                                                                                                                                               | 1   | 数値         |                     |
| payment_valid_flg                     | 状態 <br> 0:無効 <br> 1:有効                                                                                                                                                                                                                                                                                                                          | 1   | 数値         | 必須                 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                                                                                                                                    | 型            |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| user_id                      | ユーザーID                                                                                                                               | string        |
| access_key                   | アクセスキー                                                                                                                             | string        |
| limit_count                  | 入金情報取得件数 <br> ※最大件数は、リクエストで指定された「入金情報取得件数」に依存                                                         | int           |
| page_count                   | 入金情報取得開始インデックス <br> ※取得した入金情報の開始インデックスを返却する                                                             | int           |
| total_page_count             | 入金情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な入金情報の全件数／入金情報取得件数によって、算出される値を返却する   | int           |
| [payment](#payment-response) | 入金情報に属するパラメータ                                                                                                                | `array` |

#### payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                  | 概要                                                                                                                                                                                                                                                                                                                                                 | 型             |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| error_code                            | エラーコード <br> ※正常時はnull                                                                                                                                                                                                                                                                                                                      | string        |
| error_message                         | エラーメッセージ <br> ※正常時はnull                                                                                                                                                                                                                                                                                                                  | string        |
| payment_id                            | 入金ID                                                                                                                                                                                                                                                                                                                                               | int           |
| payment_method                        | 決済方法 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP口座振替 <br> 4:RL口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段1 <br> 11:その他決済手段2 <br> 12:その他決済手段3 <br> 13:その他決済手段4 <br> 14:その他決済手段5 | int           |
| deposit_date                          | 入金日                                                                                                                                                                                                                                                                                                                                               | string        |
| account_number                        | 口座番号                                                                                                                                                                                                                                                                                                                                             | string        |
| account_name                          | 振込名義                                                                                                                                                                                                                                                                                                                                             | string        |
| deposit_amount                        | 金額                                                                                                                                                                                                                                                                                                                                                 | int           |
| unclearing_amount                     | 未消込金額                                                                                                                                                                                                                                                                                                                                           | int           |
| deposit_import_reserve_date           | 取込日                                                                                                                                                                                                                                                                                                                                               | string        |
| deposit_import_reserve_format_options | 取込形式 <br> 1:全銀 <br> 2:CSV-銀行振込 <br> 3:金融機関連携 <br> 5:CSV-その他 <br> 6:API                                                                                                                                                                                                                                                              | int           |
| clearing_method                       | 消込方法 <br> 0:自動消込 <br> 1:手動消込                                                                                                                                                                                                                                                                                                              | int           |
| clearing_status                       | 入金消込ステータス <br> 0:未消込み <br> 1:完了 <br> 2:確認済み                                                                                                                                                                                                                                                                                         | int           |
| suspense_name                         | 仮受先名                                                                                                                                                                                                                                                                                                                                             | string        |
| clearing_transfer_status_options      | 振替区分 <br> 0:仮受 <br> 1:入金                                                                                                                                                                                                                                                                                                                      | int          |
| clearing_key                          | 消込キー                                                                                                                                                                                                                                                                                                                                             | string        |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": "",
    "page_count": "",
    "payment": 
        {
            "payment_id": "123456",
            "billing_name": "振込名義",
            "payment_method": "1",
            "payment_transfer_date_from": "2020/03/01",
            "payment_transfer_date_to": "2020/04/11",
            "payment_clearing_status": "0",
            "clearing_transfer_status_options": "1",
            "deposit_import_reserve_date_from": "2020/02/01",
            "deposit_import_reserve_date_to": "2020/03/31",
            "deposit_import_reserve_format_options": "1",
            "payment_clearing_method_options": "0",
            "payment_valid_flg": "1"
        }
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 0,
    "total_page_count": 0,
    "payment": [
        {
            "error_code": "null",
            "error_message": "null",
            "payment_id": 123456,
            "deposit_payment_method":1 ,
            "payment_date": "2020/04/01",
            "account_number": "1234abc",
            "deposit_bill_account_name": "振込名義",
            "deposit_amount": 5000 ,
            "unclearing_amount": 0 ,
            "deposit_import_reserve_date": "2020/04/01",
            "deposit_import_reserve_format_options": 1,
            "clearing_method": 0,
            "clearing_status": 0,
            "suspense_name": "株式001",
            "clearing_transfer": 0,
            "clearing_key":""
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード  | 内容                                       |
| ------------ | ------------------------------------------ |
| 3501         | 入金IDが不正                                |
| 3502         | 相手先が不正                                |
| 3503         | 決済方法が不正                              |
| 3504         | 決済日の開始日が不正                        |
| 3505         | 決済日の終了日が不正                        |
| 3506         | 入金消込ステータスが不正                     |
| 3507         | 振替区分が不正                              |
| 3508         | 取込日の開始日が不正                         |
| 3509         | 取込日の終了日が不正                         |
| 3510         | 取込形式が不正                              |
| 3511         | 消込方法が不正                              |
| 3512         | 状態が不正                                  |
| 3513         | 入金情報取得件数が不正                       |
| 3514         | 入金情報取得開始インデックスが不正            |
| 3515         | リクエストパラメータに入金情報が存在しません。 |

----

[TOPへ戻る](../../index.md)