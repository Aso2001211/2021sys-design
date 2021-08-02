@startuml
ユーザー -> Webサーバー : ログイン
Webサーバー -> DBサーバー:ログイン照会
DBサーバー -> DBサーバー:ログイン処理
DBサーバー -> Webサーバー:認証結果
Webサーバー -> ユーザー:認証結果
activate ユーザー
@enduml
