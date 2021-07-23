---
title: "【画像で説明】AlfredからGoogleTasks(ToDo)にタスクを追加するWorkflowをPythonで作る"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python, mac, alfred, google, api]
published: true
---

# やりたいこと

こうしたら


![スクリーンショット 2020-11-07 4 52 58" ](https://user-images.githubusercontent.com/26497221/98408891-4395eb00-20b5-11eb-9dab-7124cbb95c4f.png)

---
**⇣**
---

こうっ

![スクリーンショット 2020-11-07 4 53 09" ](https://user-images.githubusercontent.com/26497221/98408947-5c060580-20b5-11eb-9f9d-e3e6e6d8b322.png)

---

こうしたら

![スクリーンショット 2020-11-07 4 53 23" ](https://user-images.githubusercontent.com/26497221/98408951-5f00f600-20b5-11eb-8d97-d1f4ffa03c9d.png)

---
**⇣**
---

こうっ

![スクリーンショット 2020-11-07 4 53 34" ](https://user-images.githubusercontent.com/26497221/98409000-75a74d00-20b5-11eb-8c43-bb32b237d69e.png)


# 前提条件
* Mac OS
* Alfredのインストール (https://www.alfredapp.com/)
* Alfred Powerpack購入済 (https://www.alfredapp.com/shop/)
* Googleアカウント作成済

今回説明するworkflowは無料版では使えない機能のため、Powerpackの購入が必要となります。
Powerpackの料金は2020-11-15現在のレートで約¥6,770でした。


# GCPアカウントの作成
Google Tasks APIを使うためにGCPアカウントが必要になります。

[無料アカウントの作成方法](https://qiita.com/komiya_____/items/14bd06d0866f182ae912)

# 認証準備
## プロジェクト作成 ~ 認証キー取得まで

### プロジェクト作成
![スクリーンショット 2020-11-07 2 14 20" ](https://user-images.githubusercontent.com/26497221/98395228-4d145880-209f-11eb-8fc3-ee0c9f7c4474.png)

![スクリーンショット 2020-11-07 2 14 43" ](https://user-images.githubusercontent.com/26497221/98395238-50a7df80-209f-11eb-9598-90a79ad006f4.png)

### Tasks APIの有効化
![スクリーンショット 2020-11-07 2 11 45" ](https://user-images.githubusercontent.com/26497221/98395847-2efb2800-20a0-11eb-8aff-b7d4a7ffba49.png)

![スクリーンショット 2020-11-07 2 24 01" ](https://user-images.githubusercontent.com/26497221/98396103-94e7af80-20a0-11eb-996e-898bcbbfe4b9.png)

![スクリーンショット 2020-11-07 2 24 26" ](https://user-images.githubusercontent.com/26497221/98396095-91ecbf00-20a0-11eb-9eba-c5f715bb919e.png)

### OAuth2.0クライアントIDの作成
![スクリーンショット 2021-07-23 14 36 17](https://user-images.githubusercontent.com/26497221/126741341-48119138-6758-45b2-8046-ae6156840412.png)

同意画面の設定が必要な場合があります。
(以下の画面が出なければ飛ばしてOK)

![スクリーンショット 2021-07-23 14 42 22](https://user-images.githubusercontent.com/26497221/126743486-360b1836-2bdf-4d4b-ae8e-ba605e04f329.png)

![スクリーンショット 2021-07-23 14 43 56](https://user-images.githubusercontent.com/26497221/126743521-8cfcd555-80d9-4ec3-9b9e-8aefa1a7e83d.png)

![スクリーンショット 2021-07-23 14 44 55](https://user-images.githubusercontent.com/26497221/126743555-a5c17572-1b9a-4928-af33-2cd36a38bc56.png)

再度`+ 認証情報を作成`へ

![スクリーンショット 2021-07-23 14 36 17](https://user-images.githubusercontent.com/26497221/126741341-48119138-6758-45b2-8046-ae6156840412.png)

![スクリーンショット 2020-11-07 2 38 46" ](https://user-images.githubusercontent.com/26497221/98397570-e5600c80-20a2-11eb-9a68-c60193de4484.png)

<br>

こんな画面が出てきますが、この情報は今回は不要です。

![スクリーンショット 2020-11-07 2 39 14" ](https://user-images.githubusercontent.com/26497221/98397585-e98c2a00-20a2-11eb-86b8-7d204cc7fd5d.png)

### 認証キー(credentials.json)のダウンロード

![スクリーンショット 2020-11-07 2 39 39" ](https://user-images.githubusercontent.com/26497221/98397590-ed1fb100-20a2-11eb-8915-282bc689da06.png)


# AlfredのWorkflowを作る

ここからはAlfredの設定画面をさわっていきます。


![スクリーンショット 2020-11-06 20 24 49](https://user-images.githubusercontent.com/26497221/98363618-9c905f80-2072-11eb-94ab-e9707fb8ae4a.png)

![スクリーンショット 2020-11-06 20 28 39](https://user-images.githubusercontent.com/26497221/98363765-da8d8380-2072-11eb-86f4-3634a865f1df.png)

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

![スクリーンショット 2020-11-07 4 30 23" ](https://user-images.githubusercontent.com/26497221/98407470-18aa9780-20b3-11eb-89cf-3eb4bc7ca39b.png)

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

![スクリーンショット 2020-11-07 4 30 23" ](https://user-images.githubusercontent.com/26497221/98407470-18aa9780-20b3-11eb-89cf-3eb4bc7ca39b.png)

成功。

![スクリーンショット 2020-11-07 4 44 01" ](https://user-images.githubusercontent.com/26497221/98408461-860af800-20b4-11eb-8f7d-afc2c22354a2.png)

Oauth認証は2回目以降は不要です。
初回認証時に `token.pickle`というファイルが生成され、認証情報が保存されているようです。

# 参照
ありがとうございました。

https://webrandum.net/alfred4-how-to-create-workflow/

https://developers.google.com/tasks/quickstart/python
