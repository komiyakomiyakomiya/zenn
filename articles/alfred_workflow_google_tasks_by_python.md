---
title: "【画像で説明】PythonでAlfredからGoogleTasks(ToDo)にタスクを追加するWorkflowを作る"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python, mac, alfred, google, api]
published: true
---

# やりたいこと

こうしたら


<img width="1115" alt="スクリーンショット 2020-11-07 4 52 58" src="https://user-images.githubusercontent.com/26497221/98408891-4395eb00-20b5-11eb-9dab-7124cbb95c4f.png">

こうっ

<img width="1115" alt="スクリーンショット 2020-11-07 4 53 09" src="https://user-images.githubusercontent.com/26497221/98408947-5c060580-20b5-11eb-9f9d-e3e6e6d8b322.png">

こうしたら

<img width="1115" alt="スクリーンショット 2020-11-07 4 53 23" src="https://user-images.githubusercontent.com/26497221/98408951-5f00f600-20b5-11eb-8d97-d1f4ffa03c9d.png">

こうっ

<img width="1115" alt="スクリーンショット 2020-11-07 4 53 34" src="https://user-images.githubusercontent.com/26497221/98409000-75a74d00-20b5-11eb-8c43-bb32b237d69e.png">







# GCPアカウントの作成
Google Tasks APIを使うためにGCPアカウントが必要になります。

[無料アカウントの作成方法](https://qiita.com/komiya_____/items/14bd06d0866f182ae912)

# 認証準備

## Quickstartをつかう方法

https://developers.google.com/tasks/quickstart/python

こちらのクイックスタートを利用すると、

  * GCPプロジェクトの作成
  * Tasks APIの有効化
  * OAuth2.0クライアントIDの作成
  * 認証キー(credentials.json)のダウンロード

をまとめて行うことができます。

![スクリーンショット 2020-11-06 22 26 18](https://user-images.githubusercontent.com/26497221/98392717-ad090000-209b-11eb-8d7b-e583563ec35b.png)


![スクリーンショット 2020-11-06 22 34 13](https://user-images.githubusercontent.com/26497221/98393020-125cf100-209c-11eb-8e67-7e000f779967.png)

とりあえずDesktop appで問題ありませんでした。

![スクリーンショット 2020-11-06 22 34 47](https://user-images.githubusercontent.com/26497221/98393045-1ab52c00-209c-11eb-8877-dcf43d60c025.png)

<img width="1117" alt="スクリーンショット 2020-11-07 2 02 59" src="https://user-images.githubusercontent.com/26497221/98394026-80ee7e80-209d-11eb-85ec-312f823d054c.png">

## 通常のプロジェクト作成 ~ 認証キー取得まで

### プロジェクト作成
<img width="1169" alt="スクリーンショット 2020-11-07 2 14 20" src="https://user-images.githubusercontent.com/26497221/98395228-4d145880-209f-11eb-8fc3-ee0c9f7c4474.png">
<img width="766" alt="スクリーンショット 2020-11-07 2 14 43" src="https://user-images.githubusercontent.com/26497221/98395238-50a7df80-209f-11eb-9598-90a79ad006f4.png">

### Tasks APIの有効化
<img width="1169" alt="スクリーンショット 2020-11-07 2 11 45" src="https://user-images.githubusercontent.com/26497221/98395847-2efb2800-20a0-11eb-8aff-b7d4a7ffba49.png">

<img width="1176" alt="スクリーンショット 2020-11-07 2 24 01" src="https://user-images.githubusercontent.com/26497221/98396103-94e7af80-20a0-11eb-996e-898bcbbfe4b9.png">

<img width="925" alt="スクリーンショット 2020-11-07 2 24 26" src="https://user-images.githubusercontent.com/26497221/98396095-91ecbf00-20a0-11eb-9eba-c5f715bb919e.png">

### OAuth2.0クライアントIDの作成

<img width="1171" alt="スクリーンショット 2020-11-07 2 29 16" src="https://user-images.githubusercontent.com/26497221/98397551-ded19500-20a2-11eb-8ac2-326cae359253.png">

<img width="1171" alt="スクリーンショット 2020-11-07 2 38 46" src="https://user-images.githubusercontent.com/26497221/98397570-e5600c80-20a2-11eb-9a68-c60193de4484.png">

<br>

こんな画面が出てきますが、この情報は今回は不要です。

<img width="528" alt="スクリーンショット 2020-11-07 2 39 14" src="https://user-images.githubusercontent.com/26497221/98397585-e98c2a00-20a2-11eb-86b8-7d204cc7fd5d.png">

### 認証キー(credentials.json)のダウンロード

<img width="1176" alt="スクリーンショット 2020-11-07 2 39 39" src="https://user-images.githubusercontent.com/26497221/98397590-ed1fb100-20a2-11eb-8915-282bc689da06.png">


# AlfredのWorkflowを作る

ここからはAlfredの設定画面をさわっていきます。

![スクリーンショット 2020-11-06 21 01 07](https://user-images.githubusercontent.com/26497221/98365278-859f3c80-2075-11eb-8fe9-73d40d6c8de9.png)


![スクリーンショット 2020-11-06 20 28 39](https://user-images.githubusercontent.com/26497221/98363765-da8d8380-2072-11eb-86f4-3634a865f1df.png)

![スクリーンショット 2020-11-06 20 24 49](https://user-images.githubusercontent.com/26497221/98363618-9c905f80-2072-11eb-94ab-e9707fb8ae4a.png)

⇣ ひとまずNameだけあれば大丈夫です。

![スクリーンショット 2020-11-06 21 01 07](https://user-images.githubusercontent.com/26497221/98364447-2a207f00-2074-11eb-92b2-4cd7a14268ce.png)

`/Users/ユーザー名/Library/Application Support/Alfred/Alfred.alfredpreferences/workflows/` の中に `user.workflow.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/`みたいなディレクトリが作られます。
このディレクトリがAlfredからPythonファイルを実行するさいのカレントディレクトリになります。


# Pythonファイルの作成

先程の`user.workflow.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/`ディレクトリの中に好きな名前でファイルを作ります。
今回は`add_task.py`というファイルで。

```sh
$ cd /Users/ユーザー名/Library/Application Support/Alfred/Alfred.alfredpreferences/workflows/user.workflow.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

$ touch add_task.py
```

さきほどダウンロードした `credentials.json` も `add_task.py` と同じディレクトリに置きます。

# ライブラリのインストール

* `google-api-python-client`
* `google-auth-httplib2 `
* `google-auth-oauthlib`

の3つが必要。

```sh
$ pip install --target=. google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

`user.workflow.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/`ディレクトリの中で、pip installします。
`--target`オプションで`.(カレントディレクトリ)`を指定することで、ディレクトリ内に依存ライブラリを含む一式をインストールできます。
このやり方でないと、not found的なエラーで一生前に進めませんでした。
仮想環境を使うやり方などありましたら教えて下さい🙇

# コード

クイックスタートのコードをベースにしています。

```py
from __future__ import print_function
import pickle
import os.path
import sys
from googleapiclient.discovery import build
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request

SCOPES = ['https://www.googleapis.com/auth/tasks']


def main():
    """Shows basic usage of the Tasks API.
    Prints the title and ID of the first 10 task lists.
    """
    creds = None
    # The file token.pickle stores the user's access and refresh tokens, and is
    # created automatically when the authorization flow completes for the first
    # time.
    if os.path.exists('token.pickle'):
        with open('token.pickle', 'rb') as token:
            creds = pickle.load(token)
    # If there are no (valid) credentials available, let the user log in.
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                './credentials.json', SCOPES)
            creds = flow.run_local_server(port=0)
        # Save the credentials for the next run
        with open('token.pickle', 'wb') as token:
            pickle.dump(creds, token)

    service = build('tasks', 'v1', credentials=creds)

    # Call the Tasks API
    results = service.tasklists().list(maxResults=10).execute()
    items = results.get('items', [])

    title = sys.argv[1]

    if items:
        task = {
            'title': title,
        }
        result = service.tasks().insert(tasklist='@default', body=task).execute()


if __name__ == '__main__':
    main()
```

以下、主な変更点です。

タスク追加用にスコープを変更しています。

```py
...
SCOPES = ['https://www.googleapis.com/auth/tasks.readonly']
⇣
SCOPES = ['https://www.googleapis.com/auth/tasks']
...
```

コマンドライン引数でタスク名を渡し、タスクに追加するように改変しています。

```py
title = sys.argv[1]

if items:
    task = {
        'title': title,
    }
    result = service.tasks().insert(tasklist='@default', body=task).execute(
```


# Workflowの中身

![スクリーンショット 2020-11-07 3 18 15](https://user-images.githubusercontent.com/26497221/98404629-49d49900-20ae-11eb-8785-8f70934f7bfe.png)

Keywordを指定

![スクリーンショット 2020-11-07 3 18 31](https://user-images.githubusercontent.com/26497221/98404648-55c05b00-20ae-11eb-996d-c455c930f837.png)

KeywordはAlfredの先頭に打ち込む文字列。
ここでは`task hoge` でhogeがタスクとして追加されるようになる。

![スクリーンショット 2020-11-07 3 21 10](https://user-images.githubusercontent.com/26497221/98404681-61ac1d00-20ae-11eb-9068-b512d7fb7063.png)

次は、Action > Run Script

![スクリーンショット 2020-11-07 3 21 39](https://user-images.githubusercontent.com/26497221/98404729-78527400-20ae-11eb-89f4-065708ad93f0.png)

実行するPythonファイルを指定する。

![スクリーンショット 2020-11-07 4 06 21](https://user-images.githubusercontent.com/26497221/98405112-0e869a00-20af-11eb-8e6a-3b7ea42f753a.png)

ドラッグして繋げば完成

![スクリーンショット 2020-11-07 3 22 33](https://user-images.githubusercontent.com/26497221/98404761-843e3600-20ae-11eb-864a-a5a6b8638c2b.png)


# 動作確認

試してみる。

<img width="1120" alt="スクリーンショット 2020-11-07 4 30 23" src="https://user-images.githubusercontent.com/26497221/98407470-18aa9780-20b3-11eb-89cf-3eb4bc7ca39b.png">

初回は認証が必要。

![スクリーンショット 2020-11-07 4 28 29](https://user-images.githubusercontent.com/26497221/98407503-23652c80-20b3-11eb-8dc1-b54561e35aff.png)

![スクリーンショット 2020-11-07 4 31 01](https://user-images.githubusercontent.com/26497221/98408222-29a7d880-20b4-11eb-892d-e1a9c2089493.png)

![スクリーンショット 2020-11-07 4 31 07](https://user-images.githubusercontent.com/26497221/98408241-2f052300-20b4-11eb-9138-54d38cb2d8d2.png)

許可で。

![スクリーンショット 2020-11-07 4 31 31](https://user-images.githubusercontent.com/26497221/98407541-324bdf00-20b3-11eb-9e63-4fd3cfb1932c.png)

許可で。

![スクリーンショット 2020-11-07 4 31 48](https://user-images.githubusercontent.com/26497221/98407553-37109300-20b3-11eb-8f79-c8fbf6c425b6.png)

こんな画面になったら認証完了。
閉じる。

![スクリーンショット 2020-11-07 4 32 06](https://user-images.githubusercontent.com/26497221/98407575-3c6ddd80-20b3-11eb-8638-1f6eaf193faa.png)


改めまして

<img width="1120" alt="スクリーンショット 2020-11-07 4 30 23" src="https://user-images.githubusercontent.com/26497221/98407470-18aa9780-20b3-11eb-89cf-3eb4bc7ca39b.png">

成功。

<img width="1115" alt="スクリーンショット 2020-11-07 4 44 01" src="https://user-images.githubusercontent.com/26497221/98408461-860af800-20b4-11eb-8f7d-afc2c22354a2.png">

Oauth認証は2回目以降は不要です。
初回認証時に `token.pickle`というファイルが生成され、認証情報が保存されているようです。

# 参照
ありがとうございました。

https://webrandum.net/alfred4-how-to-create-workflow/

https://developers.google.com/tasks/quickstart/python
