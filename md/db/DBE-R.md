
```uml

@startuml

/'
  図の中で目立たせたいエンティティに着色するための
  色の名前（定数）を定義できます。
  https://plantuml.com/ja/skinparam
'/

!define MASTER_MARK_COLOR Orange 
!define TRANSACTION_MARK_COLOR DeepSkyBlue

'グラデーションさせる場合 #xx-xx
!define MAIN_ENTITY #MintCream-MistyRose

/'
  デフォルト色を"skinparam class"で設定します。
'/
skinparam class {
    '図の背景
    BackgroundColor Snow
    '図の枠
    BorderColor Black
    'リレーションの色
    ArrowColor Black
}

package "ECサイト" as target_system {
    /'
      マスターテーブルを M、トランザクションを T などで表記
      １文字なら "主" とか "従" まど日本語でも記載可能
     '/

    entity "顧客マスタ" as customer <m_customers> <<M,MASTER_MARK_COLOR>> {
        + customer_code [PK]
        --
        pass
        name
        address
        tel
        mail
        del_flag
        reg_date
    }

    entity "購入テーブル" as order <d_purchase> <<T,TRANSACTION_MARK_COLOR>> MAIN_ENTITY {
        + order_id [PK]
        --
        # customer_code [FK]
        purchase_date
        total_price
    }


    entity "購入詳細テーブル" as order_detail <d_purchase_detail> <<T,TRANSACTION_MARK_COLOR>> MAIN_ENTITY {
        + order_id   [PK]
        + detail_id  [PK]
        --
        # item_code [FK]
        price
        num
    }


    entity "商品マスタ" as items <m_items> <<M,MASTER_MARK_COLOR>> {
        + item_code [PK]
        --
        item_name
        price
        # category_id [FK]
        image
        detail
        del_flag
        reg_date
    }


    entity "カテゴリマスタ" as category <m_category> <<M,MASTER_MARK_COLOR>> {
        + category_id [PK]
        --
        name
        reg_date
    }


}

/'
  テーブルのつながりの記載方法
  https://qiita.com/murakami-mm/items/4c50d1949a8b10016ef7

    ------   :1
    ----||   :1 and only 1
    ----o|   :0 or 1
    -----{   :many
    ----|{   :1 or more (1以上)
    ----o{   :0 or many (0以上)

これだと縦につながる
customer       |o--o{     order
order          ||--|{     order_detail
order_detail    }--||     items
items          }o--||     category

 left    le
 right   ri
 up      up
 down    do


'/
customer       |o-ri-o{     order
order          ||-ri-|{     order_detail
order_detail    }-do-||     items
items          }o-le-||     category


@enduml

```
