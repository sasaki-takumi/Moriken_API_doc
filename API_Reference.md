# もりけんAPIリファレンス

## Common Specification
### Responce Format

- 全てのAPIはデータをJSON形式で返します。
- 全てのAPIにおいて、メタ情報（URL, method）を返します。
- レスポンスには後述するステータスコードとそれに対応するメッセージが付与されます。
- エラーの場合はデータがないときなどのエラーと、バリデーションエラーの2種類のレスポンスがあります。
- Success以外の場合はメッセージ内容とコード以外は全て同じフォーマットになります。(そのため各APIのReferenceのExample ResponseにはSuccessのみ示します)

#### Success

```
{
    "meta": {
        "url": "url",
        "method": "method"
    },
    "response": {
        "code": 200,
        "message": "message",
        "modelname(s)": array()
    }
}
```

#### Error

```
{
    "meta": {
        "url": "url",
        "method": "method"
    },
    "error": {
        "code": (int)code,
        "message": "message",
    }
}
```

#### Validation Error

```
{
    "meta": {
        "url": "url",
        "method": "method"
    },
    "error": {
        "code": (int)code,
        "message": "Validation Error",
        "validation": {
            "ModelName": {
                "columName": "message",
                        .
                        .
                        .
            }
        }
    }
}
```

### Status Code

|ステータスコード|メッセージ|
|:------------|:---|
|200（OK）|リクエストに成功しました。|
|201（Created）|作成に成功しました。|
|400（BadRequest）|未入力の項目があるか、入力内容が間違っています。|
|401（AuthorizationRequired）|認証が失敗しているか、未認証の状態です。|
|403（Forbidden）|権限がありません。|
|404（NotFound）|リソースが存在しません。|
|409（Conflict）|重複しています。|
|500（InternalServerError）|サーバエラーです。|
|503（ServiceUnavailable）|メンテナンス中か、サーバがダウンしています。|

## UserAdd API

### Summary
ユーザの登録APIです。
Twitter, Facebookでの登録も可能です。
Twitter, Facebookからはユーザ名，アイコンを取得します。

### Resource URL
http://sakumon.jp/app/LK_API/users.json

### Resource Information
- Method: POST
- Contoroller#action: users#add
- Requires Authentication: No

### Request Parameter

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|username|ユーザ名|text|◯|
|email|メールアドレス|text|◯|
|password|パスワード|text|◯SNS認証以外は必須)|
|image|アイコン画像|text||
|twitter_id|ツイッターID|int||
|twitter_access_token|ツイッターアクセストークン|text||
|twitter_access_token_secret|ツイッターシークレットトークン|text||
|facebook_id|FacebookID|int||
|facebook_access_token|Facebookアクセストークン|text||

### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|ステータスコード|int|
|message|メッセージ|text|
|id|ユーザID|int|
|username|ユーザ名|text|
|password|パスワード|text|
|email|メールアドレス|text|
|image|ファイルパス|text|
|twitter_id|TwitterのID|int|
|twitter_access_token|Twitterのアクセストークン|text|
|twitter_access_token_secret|Twitterのシークレットトークン|text|
|facebook_id|FacebookのID|int|
|facebook_access_token|Facebookのアクセストークン|text|
|token|ユーザのトークン|text|
|created|ユーザ作成日|datetime|
|modified|ユーザ編集日|datetime|

### Example Request
POST<br />
http://sakumon.jp/app/LK_API/users.json

```
array(
    'User' => array(
        'username' => 'test',
        'email' => 'test@test.com',
        'password' => 'password'
    )
)
```

### Example Response

```
{
    "meta": {
        "url": "/LK_API/users.json",
        "method": "POST"
    },
    "response": {
        "code": 201,
        "message": "ユーザ登録に成功しました。",
        "user": {
            "id": "1215",
            "username": "apiss",
            "email": "api@api.com",
            "image": "filepath.jpg",
            "twitter_id": "twitter_id",
            "twitter_access_token": "token",
            "twitter_access_token_secret": "token",
            "facebook_id": "id",
            "facebook_access_token": "token",
            "token": "b0rajjdslkajdsksdkjdlkdjsakljdsl;j",
            "created": datetime,
            "modified": datetime
        }
    }
}
```

## Login API
### Summary
ユーザのログインAPIです。
TwitterやFacebookからもログイン可能です．

### Resource URL
logins.json

### Resource Information
- Method: POST
- Contoroller#action: logins#add
- Requires Authentication: No

### Request Parameter

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|username|ログイン用ユーザ名|text|◯（emailといずれか）|
|email|ログイン用メールアドレス|text|◯（usernameといずれか）|
|password|パスワード|text|◯(SNS認証以外は必須)|
|twitter_access_token|Twitterアクセストークン|text||
|facebook_access_token|Facebookアクセストークン|text||

### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|ステータスコード|int|
|message|メッセージ|text|
|id|ユーザID|int|
|username|ユーザ名|text|
|email|メールアドレス|text|
|image|ファイルパス|text|
|twitter_id|TwitterのID|int|
|twitter_access_token|Twitterのアクセストークン|text|
|twitter_access_token_secret|Twitterのシークレットトークン|text|
|facebook_id|FacebookのID|int|
|facebook_access_token|Facebookのアクセストークン|text|
|token|ユーザのトークン|text|
|created|ユーザ作成日|datetime|
|modified|ユーザ編集日|datetime|

### Example Request
POST<br />
http://sakumon.jp/app/LK_API/logins.json

```
array(
    'User' => array(
        'username' => 'test',
        'password' => 'password'
    )
)
```

### Example Responce

```
{
    "meta": {
        "url": "/LK_API/users.json",
        "method": "POST"
    },
    "response": {
        "code": 201,
        "message": "ログインに成功しました。",
        "user": {
            "id": "1215",
            "username": "apiss",
            "password": "apis",
            "email": "api@api.com",
            "image": "filepath.jpg",
            "twitter_id": "twitter_id",
            "twitter_access_token": "token",
            "twitter_access_token_secret": "token",
            "facebook_id": "id",
            "facebook_access_token": "token",
            "token": "b0rajjdslkajdsksdkjdlkdjsakljdsl;j",
            "created": datetime,
            "modified": datetime
        }
    }
}
```

## Index Problems API

### Summary
- 過去問題を取得するAPIです。
- n問(100問未満)取得はランダムに取得します。
- 全問(100問)取得は過去問の場合設問番号の昇順。オリジナル問題は登録順。
- 一度に100問以上の問題を取得することは出来ません。

### Resource URL
Problem.json

### Resource Information
- Method: GET
- Contoroller#action: Problems#index
- Requires Authentication: No

### Request Parameter

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|employ|年度(0:オリジナル問題,0以外:過去問対象西暦)|int|◯|
|category_id|カテゴリID|int|級かカテゴリIDどちらか一つが◯<br />(過去問抽出時)<br />オリジナル問題抽出時は常に◯|
|grade|級|int|級かカテゴリIDどちらか一つが◯<br />(過去問抽出時のみ)|
|item|問題取得数|int|◯|

### Responce Parameter
|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|

|フィールド|説明|型|
|:------------:|:----------|:---|
|problem:|||
|id|問題ID|int|
|kentei_id|どの検定の問題か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|user_id|問題作成者ID|int|
|type|問題形式（1.四択問題 2.記述式問題）|int|
|grade|過去問採用級|int|
|number|過去問設問番号|int|
|sentence|問題文|text|
|right_answer|正解選択肢or正解文字列|text|
|wrong_answer1|誤答選択肢1|text|
|wrong_answer2|誤答選択肢2|text|
|wrong_answer3|誤答選択肢3|text|
|description|解説文|text|
|other_answer|記述式問題の他の正解(カンマ区切り)|text|
|image|画像パス|text|
|latitude|経度・緯度情報|float|
|longitude|経度・緯度情報|float|
|reference|参考文献|text|
|spot_id|ガンライザー検定用スポットID|int|
|public_flag|問題の公開フラグ(非公開=0,公開=1) 過去問は公開,オリジナルは非公開|int|
|category_id|カテゴリid|int|
|subcategory_id|サブカテゴリid|int|
|created|問題作成日|datetime|
|modified|問題編集日|datetime|

|フィールド|説明|型|
|:------------:|:----------|:---|
|category:|||
|id|問題ID|int|
|kentei_id|どの検定のカテゴリーか(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|name|カテゴリー名|text|
|created|問題作成日|datetime|

### Example Request（1問取得）
http://sakumon.jp/LK_API/problems/index.json<br />
post_data:kentei_id=1&employ=2012&grade=3&category_id=1&item=1

### Example Responce（1問取得）
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/problems/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストに成功しました。",
        "Problems": [
            {
                "Problem": {
                    "id": "1384",
                    "kentei_id": "1",
                    "user_id": "7",
                    "type": "1",
                    "grade": "3",
                    "number": "4",
                    "sentence": "テスト問題",
                    "right_answer": "テスト正解",
                    "wrong_answer1": "テスト誤答1",
                    "wrong_answer2": "テスト誤答2",
                    "wrong_answer3": "テスト誤答3",
                    "description": "テスト解説",
                    "other_answer": "",
                    "image": "",
                    "latitude": "0",
                    "longitude": "0",
                    "reference": "",
                    "spot_id": "0",
                    "public_flag": "1",
                    "category_id": "1",
                    "subcategory_id": "53",
                    "employ": "2012",
                    "created": "2009-11-13 13:09:00",
                    "modified": "2009-11-13 13:09:00"
                },
                "Category": {
                    "id": "1",
                    "kentei_id": "1",
                    "name": "盛岡の文化",
                    "created": "2015-02-22 17:02:46"
                }
            }
        ]
    }
}
```

###Example Request（5問取得）
http://sakumon.jp/LK_API/problems/index.json<br />
post_data:kentei_id=1&employ=2012&grade=3&category_id=1&item=5

### Example Responce（5問取得）
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/problems/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストに成功しました。",
        "Problems": [
            {
                "Problem": {
                    "id": "1384",
                    "kentei_id": "1",
                    "user_id": "7",
                    "type": "1",
                    "grade": "3",
                    "number": "4",
                    "sentence": "テスト問題",
                    "right_answer": "テスト正解",
                    "wrong_answer1": "テスト誤答1",
                    "wrong_answer2": "テスト誤答2",
                    "wrong_answer3": "テスト誤答3",
                    "description": "テスト解説",
                    "other_answer": "",
                    "image": "",
                    "latitude": "0",
                    "longitude": "0",
                    "reference": "",
                    "spot_id": "0",
                    "public_flag": "1",
                    "category_id": "1",
                    "subcategory_id": "53",
                    "employ": "2012",
                    "created": "2009-11-13 13:09:00",
                    "modified": "2009-11-13 13:09:00"
                },
                "Category": {
                    "id": "1",
                    "kentei_id": "1",
                    "name": "盛岡の文化",
                    "created": "2015-02-22 17:02:46"
                }
            },

        ・
        ・
        ・

            {
                "Problem": {
                    "id": "1460",
                    "kentei_id": "1",
                    "user_id": "7",
                    "type": "1",
                    "grade": "3",
                    "number": "84",
                    "sentence": "テスト問題",
                    "right_answer": "テスト正解",
                    "wrong_answer1": "テスト誤答1",
                    "wrong_answer2": "テスト誤答2",
                    "wrong_answer3": "テスト誤答3",
                    "description": "テスト解説",
                    "other_answer": "",
                    "image": "",
                    "latitude": "0",
                    "longitude": "0",
                    "reference": "",
                    "spot_id": "0",
                    "public_flag": "1",
                    "category_id": "1",
                    "subcategory_id": "16",
                    "employ": "2012",
                    "created": "2009-11-14 14:01:00",
                    "modified": "2009-11-14 14:01:00"
                },
                "Category": {
                    "id": "1",
                    "kentei_id": "1",
                    "name": "盛岡の文化",
                    "created": "2015-02-22 17:02:46"
                }
            }
        ]
    }
}
```

###Example Request(100問取得)
http://sakumon.jp/LK_API/problems/index.json<br />
post_data:kentei_id=1&employ=2012&grade=3&item=100

### Example Responce(100問取得)
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/problems/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストに成功しました。",
        "Problems": [
            {
                "Problem": {
                    "id": "1379",
                    "kentei_id": "1",
                    "user_id": "7",
                    "type": "1",
                    "grade": "3",
                    "number": "1",
                    "sentence": "テスト問題",
                    "right_answer": "テスト正解",
                    "wrong_answer1": "テスト誤答1",
                    "wrong_answer2": "テスト誤答2",
                    "wrong_answer3": "テスト誤答3",
                    "description": "テスト解説",
                    "other_answer": "",
                    "image": "",
                    "latitude": "0",
                    "longitude": "0",
                    "reference": "",
                    "spot_id": "0",
                    "public_flag": "1",
                    "category_id": "1",
                    "subcategory_id": "53",
                    "employ": "2012",
                    "created": "2009-11-13 13:09:00",
                    "modified": "2009-11-13 13:09:00"
                },
                "Category": {
                    "id": "1",
                    "kentei_id": "1",
                    "name": "盛岡の文化",
                    "created": "2015-02-22 17:02:46"
                }
            }

        ・
        ・
        ・

            {
                "Problem": {
                    "id": "1475",
                    "kentei_id": "1",
                    "user_id": "7",
                    "type": "1",
                    "grade": "3",
                    "number": "100",
                    "sentence": "テスト問題",
                    "right_answer": "テスト正解",
                    "wrong_answer1": "テスト誤答1",
                    "wrong_answer2": "テスト誤答2",
                    "wrong_answer3": "テスト誤答3",
                    "description": "テスト解説",
                    "other_answer": "",
                    "image": "",
                    "latitude": "0",
                    "longitude": "0",
                    "reference": "",
                    "spot_id": "0",
                    "public_flag": "1",
                    "category_id": "1",
                    "subcategory_id": "30",
                    "employ": "2012",
                    "created": "2009-11-14 14:01:00",
                    "modified": "2009-11-14 14:01:00"
                },
                "Category": {
                    "id": "1",
                    "kentei_id": "1",
                    "name": "盛岡の文化",
                    "created": "2015-02-22 17:02:46"
                }
            }
        ]
    }
}
```

## Making Question API

### Summary
問題を作成するAPIです。

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action: Problems#add
- Requires Authentication: No

### Request Parameter

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の問題か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|user_id|問題作成者ID|int|◯|
|type|問題形式（1.四択問題 2.記述式問題）|int|◯|
|grade|過去問採用級|int|過去問作成時は◯|
|number|過去問設問番号|int|過去問作成時は◯|
|sentence|問題文|text|◯|
|right_answer|正解選択肢or正解文字列|text|◯|
|wrong_answer1|誤答選択肢1|text|四択問題時は◯|
|wrong_answer2|誤答選択肢2|text|四択問題時は◯|
|wrong_answer3|誤答選択肢3|text|四択問題時は◯|
|description|解説文|text|◯|
|other_answer|記述式問題の他の正解(カンマ区切り)|text||
|image|画像パス|text||
|latitude|経度・緯度情報|float||
|longitude|経度・緯度情報|float||
|spot_id|ガンライザー検定用スポットID|int||
|public_flag|問題の公開フラグ(非公開=0,公開=1) 過去問は公開,オリジナルは非公開|int|◯|
|category_id|カテゴリid|int|◯|
|subcategory_id|サブカテゴリid|int||

### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|kentei_id|どの検定の問題か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|user_id|問題作成者ID|int|
|type|問題形式（1.四択問題 2.記述式問題）|int|
|grade|過去問採用級|int|
|number|過去問設問番号|int|
|sentence|問題文|text|
|right_answer|正解選択肢or正解文字列|text|
|wrong_answer1|誤答選択肢1|text|
|wrong_answer2|誤答選択肢2|text|
|wrong_answer3|誤答選択肢3|text|
|description|解説文|text|
|other_answer|記述式問題の他の正解(カンマ区切り)|text|
|image|画像パス|text|
|latitude|経度・緯度情報|float|
|longitude|経度・緯度情報|float|
|spot_id|ガンライザー検定用スポットID|int|
|public_flag|問題の公開フラグ(非公開=0,公開=1) 過去問は公開,オリジナルは非公開|int|
|category_id|カテゴリid|int|
|subcategory_id|サブカテゴリid|int|

### Example Request（選択形式）
http://sakumon.jp/LK_API/problems/add.json<br />
post_data:kentei_id=1&user_id=1&type=1&grade=1&number=1&sentence=テスト問題文&right_answer=テスト答え&wrong_answer1=テスト誤答&wrong_answer2=テスト誤答2&wrong_answer3=テスト誤答3&description=テスト解答&public_flag&category_id=1&subcategory_id=1

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/problems/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "Problem": {
            "kentei_id": "1",
            "user_id": "1",
            "type": "1",
            "grade": "1",
            "number": "1",
            "sentence": "テスト問題文",
            "right_answer": "テスト答え",
            "wrong_answer1": "テスト誤答1",
            "wrong_answer2": "テスト誤答2",
            "wrong_answer3": "テスト誤答3",
            "description": "テスト解説",
            "public_flag": "1",
            "category_id": "1",
            "subcategory_id": "1"
        }
    }
}
```

### Example Request（一問一答形式）
http://sakumon.jp/LK_API/problems/add.json<br />
post_data:kentei_id=1&user_id=1&type=2&grade=1&number=1&sentence=テスト問題文&right_answer=テスト一問一答答え&description=テスト解答&public_flag&category_id=1&subcategory_id=1

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/problems/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "Problem": {
            "kentei_id": "1",
            "user_id": "1",
            "type": "2",
            "grade": "1",
            "number": "1",
            "sentence": "テスト問題文",
            "right_answer": "テスト一問一答答え",
            "description": "テスト解説",
            "public_flag": "1",
            "category_id": "1",
            "subcategory_id": "1"
        }
    }
}
```

## Add Answer History API

### Summary
問題解答結果を記録(answer_historisへの記録)をするAPI

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action : AnswerHistories#add

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|user_id|回答したユーザーのID|int|◯|
|problem_id|回答した問題のID|int|◯|
|answer_flag|正解したか(1:正解,2:不正解)|int|◯|
|answer_text|不正解の場合の誤回答テキスト|text||


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|int|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|user_id|回答したユーザーのID|int|
|problem_id|回答した問題のID|int|
|answer_flag|正解したか(1:正解,2:不正解)|int|
|answer_text|不正解の場合の誤回答テキスト|text||


### Example Request(success)
http://moriken_test.com/LK_API/answerHistories/add.json<br />
post_data:kentei_id=1&user_id=10&problem_id=5&answer_flag=1

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/answerHistories/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "AnswerHistory": {
            "answer_flag": "1",
            "kentei_id": "1",
            "problem_id": "5",
            "user_id": "10"
        }
    }
}
```

## Index Answer History API

### Summary
問題解答結果を取得するAPI<br />
検定,ユーザー,正解フラグを抽出条件として実行する

### Resource URL
index.json

### Resource Information
- Method: GET
- Contoroller#action : AnswerHistories#index

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|user_id|取得したい履歴のユーザーID|int|◯|
|answer_flag|正解フラグ(1:正解,2:不正解)|int||


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|user_id|指定した履歴のユーザーID|int|
|problem_id|回答した問題のID|int|
|answer_flag|正解したか(1:正解,2:不正解)|int|
|answer_text|不正解の場合の誤回答テキスト|text|
|created|履歴登録日|datetime|


### Example Request(success)
http://sakumon.jp/LK_API/answerHistories/index.json<br />
post_data:kentei_id=1&user_id=10

### Example Responce
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/answerHistories/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストに成功しました。",
        "AnswerHistories": [
            {
                "AnswerHistory": {
                    "id": "134979",
                    "kentei_id": "1",
                    "user_id": "10",
                    "problem_id": "5",
                    "answer_flag": "1",
                    "answer_text": null,
                    "created": "2015-02-07 21:56:57"
                }
            },

            ・
            ・
            ・

            {
                "AnswerHistory": {
                    "id": "134980",
                    "kentei_id": "1",
                    "user_id": "10",
                    "problem_id": "5",
                    "answer_flag": "1",
                    "answer_text": null,
                    "created": "2015-02-07 21:57:20"
                }
            }
        ]
    }
}
```

## Add Evaluate Comment API

### Summary
問題の評価内容を登録するAPI

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action : EvaluateComments#add

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|evaluate_item_id|評価対象の項目ID|int|◯|
|problem_id|評価対象の問題ID|int|◯|
|user_id|評価ユーザID|int|◯|
|evaluate_comment|評価コメント(評価者)|text|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|evaluate_item_id|評価対象の項目ID|int|
|problem_id|評価対象の問題ID|int|
|user_id|評価ユーザID|int|
|evaluate_comment|評価コメント(評価者)|text|


### Example Request(success)
http://sakumon.jp/LK_API/evaluateComments/add.json<br />
post_data:evaluate_item_id=1&problem_id=1&user_id=1&evaluate_comment=いのうえ

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/evaluateComments/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "EvaluateComment": {
            "evaluate_item_id": "1",
            "problem_id": "1",
            "user_id": "1"
            "evaluate_comment": "いのうえ",
        }
    }
}
```

## Index Evaluate Comment API

### Summary
問題の評価結果を取得するAPI

### Resource URL
index.json

### Resource Information
- Method: GET
- Contoroller#action : EvaluateComments#index

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|problem_id|評価対象の問題ID|int|problem_id,user_idのいずれか一つ以上が◯|
|user_id|評価ユーザID|int|problem_id,user_idのいずれか一つ以上が◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|evaluate_item_id|評価対象の項目ID|int|
|problem_id|評価対象の問題ID|int|
|user_id|評価ユーザID|int|
|evaluate_comment|評価コメント(評価者)|text|
|confirm_comment|確認コメント(作問者)|text|
|confirm_flag|作問者の確認フラグ(1:未確認,2:容認,3:否認)|int|
|created|評価登録日|datetime|


### Example Request(success)
http://sakumon.jp/LK_API/evaluateComments/index.json<br />
post_data:problem_id=1&user_id=1

### Example Responce
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/evaluateComments/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストの作成に成功しました。",
        "EvaluateComments": [
            {
                "EvaluateComment": {
                    "id": "1",
                    "evaluate_item_id": "1",
                    "problem_id": "1",
                    "user_id": "1"
                    "evaluate_comment": "test!",
                    "confirm_comment": "",
                    "confirm_flag": "1",
                    "created": "2015-03-16 23:57:16",
                }
            },

            ・
            ・
            ・

            {
                "EvaluateComment": {
                    "id": "1",
                    "evaluate_item_id": "1",
                    "problem_id": "1",
                    "user_id": "1"
                    "evaluate_comment": "test!2",
                    "confirm_comment": "",
                    "confirm_flag": "1",
                    "created": "2015-03-17 23:57:16",
                }
            }
        ]
    }
}
```

## Edit Evaluate Comment API

### Summary
問題の評価結果を更新するAPI<br />
評価者が評価した内容を作問者が確認(容認or否認)した結果を更新する

### Resource URL
edit.json

### Resource Information
- Method: PUT
- Contoroller#action : EvaluateComments#Edit

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|id|更新対象の評価コメントID|int|◯|
|confirm_comment|作問者からの確認コメント|text|◯|
|confirm_flag|作問者の確認フラグ(1:未確認,2:容認,3:否認)|int|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|confirm_comment|作問者からの確認コメント|text|
|confirm_flag|作問者の確認フラグ(1:未確認,2:容認,3:否認)|int|

### Example Request(success)
http://sakumon.jp/LK_API/evaluateComments/edit/1.json<br />
post_data:id=1(idは↑のurlのパラメーターとして送る),confirm_comment=testComment&confirm_flg=2

### Example Responce
```
{
    "meta": {
        "method": "PUT",
        "url": "/LK_API/evaluateComments/edit/1.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "confirm_comment": "tesComment",
        "confirm_flag": "2"
    }
}
```

## Index Evaluate Item API

### Summary
対象検定の評価項目を取得するAPI<br />

### Resource URL
index.json

### Resource Information
- Method: GET
- Contoroller#action : EvaluateItems#Index

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|name|評価項目名|text|

### Example Request(success)
http://sakumon.jp/LK_API/evaluateItems/index.json<br />
post_data:kentei_id=1

### Example Responce
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/evaluateItems/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストに成功しました。"
        "EvaluateItems": [
            {
                "EvaluateItem": {
                    "id": "1",
                    "kentei_id": "1",
                    "name": "項目名1"
                    "created": "2015-01-01 00:00:00",
                }
            },
            ・
            ・
            ・
            {
                "EvaluateItem": {
                    "id": "3",
                    "kentei_id": "1",
                    "name": "項目名2"
                    "created": "2015-01-01 00:00:00",
                }
            }
        ],
    }
}
```

## Add Category API

### Summary
カテゴリーを追加するAPI<br />

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action : Categories#Add

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|name|カテゴリー名|text|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|name|カテゴリー名|text|

### Example Request(success)
http://sakumon.jp/LK_API/categories/add.json<br />
post_data:kentei_id=1&name=盛岡の文化

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/categories/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "Category": {
            "kentei_id": "1",
            "name": "盛岡の文化"
        }
    }
}
```

## Index Category API

### Summary
カテゴリーを取得するAPI<br />
対象検定のサブカテゴリも同時に抽出する

### Resource URL
index.json

### Resource Information
- Method: GET
- Contoroller#action : Categories#Index

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|id|カテゴリーID|int|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|name|カテゴリー名|text|

### Example Request(success)
http://sakumon.jp/LK_API/categories/index.json<br />
post_data:kentei_id=1

### Example Responce
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/categories/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストに成功しました。",
        "Categories": [
            {
                "Category": {
                    "id": "9",
                    "kentei_id": "1",
                    "name": "盛岡の歴史"
                    "created": "2015-02-22 17:02:46",
                }
            },
            ・
            ・
            ・
            {
                "Category": {
                    "id": "14",
                    "kentei_id": "1",
                    "name": "盛岡の現在"
                    "created": "2015-02-22 17:02:46",
                }
            }
        ]
    }
}
```

## Add SubCategory API

### Summary
サブカテゴリーを追加するAPI<br />

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action : SubCategories#Add

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|category_id|親となるカテゴリーID|int|◯|
|name|サブカテゴリー名|text|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|category_id|親となるカテゴリーID|int|
|name|カテゴリー名|text|

### Example Request(success)
http://sakumon.jp/LK_API/subCategories/add.json<br />
post_data:category_id=2&kentei_id=3&name=盛岡の食文化

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/subcategories/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "Subcategory": {
            "kentei_id": "3",
            "category_id": "2",
            "name": "盛岡の食文化"
        }
    }
}
```

## Add Tag API

### Summary
タグを追加するAPI<br />

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action : Tags#Add

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|name|タグ名|text|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|kentei_id|どの検定の回答履歴か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|
|name|タグ名|text|

### Example Request(success)
http://sakumon.jp/LK_API/tags/add.json<br />
post_data:kentei_id=1&name=石川啄木

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/tags/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "Tag": {
            "kentei_id": "1",
            "name": "石川啄木"
        }
    }
}
```

## Add Comment API

### Summary
コメントを追加するAPI<br />

### Resource URL
add.json

### Resource Information
- Method: POST
- Contoroller#action : Comments#Add

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|target|何に対してのコメントか(1:マイページ,2:問題)|int|◯|
|from_user_id|コメントしたユーザーID|int|◯|
|to_action_id|コメントされたID(ユーザor問題)|int|◯|
|comment|コメント内容|text|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|
|target|何に対してのコメントか(1:マイページ,2:問題)|int|
|from_user_id|コメントしたユーザーID|int|
|to_action_id|コメントされたID(ユーザor問題)|int|
|comment|コメント内容|text|

### Example Request(success)
http://sakumon.jp/LK_API/comments/add.json<br />
post_data:target=2&from_user_id=1&to_action_id=2&comment=テストコメント

### Example Responce
```
{
    "meta": {
        "method": "POST",
        "url": "/LK_API/comments/add.json"
    },
    "response": {
        "code": 201,
        "message": "作成に成功しました。",
        "Comment": {
            "target": "2",
            "from_user_id": "1",
            "to_action_id": "2"
            "comment": "テストコメント",
        }
    }
}
```

## Index Comment API

### Summary
コメントを取得するAPI<br />

### Resource URL
index.json

### Resource Information
- Method: GET
- Contoroller#action : Comments#Index

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|target|何に対してのコメントか(1:マイページ,2:問題)|int|◯|
|from_user_id|コメントしたユーザーID|int||
|to_action_id|コメントされたID(ユーザor問題)|int|◯|


### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|code|APIの処理結果ステータスコード|int|
|message|APIの処理結果メッセージ|text|

|フィールド|説明|型|
|:------------:|:----------|:---|
|Comment:|||
|id|commentテーブルID|int|
|target|何に対してのコメントか(1:マイページ,2:問題)|int|
|from_user_id|コメントしたユーザーID|int|
|to_action_id|コメントされたID(ユーザor問題)|int|
|comment|コメント内容|text|

|フィールド|説明|型|
|:------------:|:----------|:---|
|User:|||
|id|userテーブルID|int|
|username|ユーザ名|text|
|image|ユーザアイコン画像|text|

### Example Request(success)
http://sakumon.jp/LK_API/comments/index.json<br />
post_data:target=1&from_user_id=2&to_action_id=2

### Example Responce
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/comments/index.json"
    },
    "response": [
        {
            "Comment": {
                "id": "13",
                "target": "1",
                "from_user_id": "2",
                "to_action_id": "2",
                "comment": "test--!"
            },
            "User": {
                "id": "2",
                "username": "テストユーザ",
                "image": "0"
            }
        },
        ・
        ・
        ・
        {
            "Comment": {
                "id": "14",
                "target": "1",
                "from_user_id": "2",
                "to_action_id": "2",
                "comment": "test--!"
            },
            "User": {
                "id": "2",
                "username": "テストユーザ",
                "image": "0"
            }
        }
    ]
}
```

## Index Levels API

### Summary
ユーザのポイントとレベルを取得するAPI

### Resource URL
index.json

### Resource Information
- Method: GET
- Contoroller#action : Levels#index

|フィールド|説明|型|必須|
|:------------:|:----------|:---|:----------:|
|kentei_id|どの検定の問題か(1:もりけんweb,2:iOSapp,3:Androidapp,4:ガンライザー検定,5:たきざわ検定web,6:たきざわ検定app)|int|◯|
|user_id|ユーザID|int|◯|

### Responce Parameter

|フィールド|説明|型|
|:------------:|:----------|:---|
|user_id|ユーザID|int|
|use_level|利用レベル|int|
|know_level|知識レベル|int|
|use_point|利用ポイント|int|
|login_point|ログインポイント|int|
|answer_point|解答ポイント|int|
|make_point|作問ポイント|int|
|evaluate_point|評価ポイント|int|


### Example Request(success)
http://sakumon.jp/LK_API/levels/index.json<br />
post_data:kentei_id=1&user_id=1

### Example Responce
```
{
    "meta": {
        "method": "GET",
        "url": "/LK_API/levels/index.json"
    },
    "response": {
        "code": 200,
        "message": "リクエストの作成に成功しました。",
        "Levels": {
            "id": "1",
            "user_id": "1",
            "use_level": "1",
            "know_level": "1"
            "use_point": "1",
            "login_point": "1",
            "answer_point": "1",
            "make_point": "1",
            "evaluate_point": "1"
        }
    }
}
```
