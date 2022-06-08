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
           "name": "banana sugimoto",
           "bithdate": "2022-01-01"
        }

+ Response 201 (application/json)
  + Body
        {
           "id": "01FZMTP19VSKWBQPXJA4GKZ2Y3",
           "message": "user successfully created"
        }

+ Response 400
  + Body
        {
           "message":"validation error",
           "errors":[
              {
                 "field":"name",
                 "messages":[
                   "cannot be empty",
                   "maximum length is 255"
                 ]
              },
              {
                 "field":"birthdate",
                 "messages":[
                   "cannot be null",
                   "format should be yyyy-MM-dd",
                   "cannot set future date"
                 ]
              }
           ]
        }

## GET /users/{id}
idに指定したユーザーの情報を取得します。

+ Parameters
  + id: 01FZMTP19VSKWBQPXJA4GKZ2Y3 - ユーザーのID。採番は26文字のULID。

+ Response 200 (application/json)
  + Body
        {
           "id": "01FZMTP19VSKWBQPXJA4GKZ2Y3",
           "name": "banana sugimoto",
           "birthdate": "2022-01-01"
        }

+ Response 404 (application/json)
  + Body
        {
            "message": "user not found"
        }

## GET /users
ユーザーを検索します。

+ Parameters
  + name (string, mandatory) - ユーザー名。前方一致で検索。
  + birthdate (string, optional) - yyyy-MM-dd形式。完全一致で検索。

+ Response 200 (application/json)
  + Body
        [
            {
                "id": "01FZMTP19VSKWBQPXJA4GKZ2Y3",
                "name": "banana sugimoto"
                "birthdate": "2022-01-01"
            }
        ]

+ Response 200 (application/json)
  + Body
        []

## PATCH /users/{id}
idに指定したユーザーの情報を更新します。

+ name (string, required) - ユーザー名。255文字以内。
+ birthdate (string, required) - 生年月日。yyyy-MM-dd形式。未来日付は不可。

+ Parameters
  + id: 01FZMTP19VSKWBQPXJA4GKZ2Y3 - ユーザーのID。採番は26文字のULID。

+ Request (application/json)
  + Body
        {
            "name": "ringo sugimoto"
            "birthdate": "2022-01-01"
        }

+ Request (application/json)
  + Body
        {
            "name": "ringo sugimoto"
        }

+ Request (application/json)
  + Body
        {
            "birthdate": "2022-01-01"
        }

+ Request (application/json)
  + Body
        {}

+ Response 200 (application/json)
  + Body
        {
            "message": "user successfully updated"
        }

+ Response 400
  + Body
        {
            "message":"validation error",
            "errors":[
               {
                  "field":"name",
                  "messages":[
                      "cannot be empty",
                      "maximum length is 255"
                  ]
               },
               {
                  "field":"birthdate",
                  "messages":[
                      "cannot be null",
                      "format should be yyyy-MM-dd",
                      "cannot set future date"
                  ]
               }
            ]
        }

+ Response 400
  + Body
        {
            "message":"validation error",
            "error":{
                "field":"name",
                "message":[
                    "cannot be empty",
                    "maximum length is 255"
                ]
            }
        }

+ Response 400
  + Body
        {
            "message":"validation error",
            "error":{
                "field":"birhdate",
                "messages":[
                    "cannot be null",
                    "format should be yyyy-MM-dd",
                    "cannot set future date"
                ]
            }
        }

+ Response 400
  + Body
        {
            "message": "invalid value"
        }               

+ Response 404 (application/json)
  + Body
        {
            "message": "user not found"
        }

## DELETE /users/{id}
idに指定したユーザーの情報を論理削除します。

+ deleted (boolean) - 削除フラグ。0:有効、1:削除。

+ Parameters
  + id: 01FZMTP19VSKWBQPXJA4GKZ2Y3 - ユーザーのID。採番は26文字のULID。

+ Response 200 (application/json)
  + Body
        {
            "message": "user successfully deleted"
        }

+ Response 404 (application/json)
        {
            "message": "resource not found"
        }

# Group Common

## 存在しないURIへのアクセス

Response `404`
```json
{
  "message": "resource not found"
}
```

## DBへの接続ができないなどのシステムエラー

Response `500`
```json
{
  "message": "system error"
}
```
