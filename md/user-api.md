FORMAT: 1A
# ユーザー管理API仕様書

# Group Users

## POST /users
ユーザーを登録します。
以下のパラメータをJSON形式で送信します。

+ name (string, required) - ユーザー名。255文字以内。
+ birthdate (string, required) - 生年月日。yyyy-MM-dd形式。未来日付は不可。

+ Request (application/json)

        {
          "name": "renato sugimoto",
          "bithdate": "2022-01-01"
        }
        