---
title: "PythonでGoogle翻訳"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["python", "gcp"]
published: true
---


# やること
```
'cat' -> 'ネコ'
```

# やり方1
翻訳したい単語や文章の数が少ない場合はこちらがお手軽です。

```sh
$ pip install googletrans
```

```py
from google.cloud import translate

s = 'cat'
translator = Translator()
response = translator.translate(s, dest="ja")
result = response.text
print(result)

# ネコ
```

## 参照

* https://github.com/ssut/py-googletrans
※ google translate apiが使われていますが、Google公式ではありません。

* https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429
※ リクエストが多すぎると制限がかかります。 (一定時間経過すると復活します。)

# やり方2
## GCP アカウント登録
[【画像で説明】Google Cloud Platform (GCP)の無料トライアルでアカウント登録](https://qiita.com/komiya_____/items/14bd06d0866f182ae912)

## Google Cloud SDK のインストール
[Google Cloud SDK のインストール ~ 初期化](https://qiita.com/komiya_____/items/5af0dcc8639fad9fee29)

## プロジェクト作成
[Google Cloud SDKでプロジェクトを作成する](https://qiita.com/komiya_____/items/fb02f38e1886db280d52)
※ 請求先アカウントの設定も忘れずに

## APIを有効化

```sh
$ gcloud projects list

$ gcloud config set project "YOUR-PROJECT-ID"
```

利用可能なAPIを一覧表示
```sh
$ gcloud services list --available

NAME                                                  TITLE
abusiveexperiencereport.googleapis.com                Abusive Experience Report API
acceleratedmobilepageurl.googleapis.com               Accelerated Mobile Pages (AMP) URL API
accessapproval.googleapis.com                         Access Approval API
accesscontextmanager.googleapis.com                   Access Context Manager API
...
```

今回はtranslate APIなのでgrepで絞り込み
```sh
$ gcloud services list --available | grep translate

translate.googleapis.com                              Cloud Translation API
```

現在のプロジェクトで有効なAPIを調べる
```sh
$ gcloud services list --enabled

NAME                              TITLE
bigquery.googleapis.com           BigQuery API
bigquerystorage.googleapis.com    BigQuery Storage API
billingbudgets.googleapis.com     Cloud Billing Budget API
cloudapis.googleapis.com          Google Cloud APIs
...
```

translate.googleapis.comがなければ有効化
```sh
$ gcloud services enable translate.googleapis.com
```

# コード

```py
from google.cloud import translate


def translate_text(text, project_id):
    client = translate.TranslationServiceClient()
    location = "global"
    parent = f"projects/{project_id}/locations/{location}"
    response = client.translate_text(
        request={
            "parent": parent,
            "contents": [text],
            "mime_type": "text/plain",  # mime types: text/plain, text/html
            "source_language_code": "en-US",  # 翻訳前の言語を指定
            "target_language_code": "ja",  # 翻訳後の言語を指定
        }
    )
    for translation in response.translations:
        result = translation.translated_text
    return result


s = 'cat'
project_id = "YOUR-PROJECT-ID"
result = translate_text(s, project_id)
print(result)

# ネコ
```

ちなみにresponseの中身はこんな構造

```py
...
def translate_text(text, project_id):
    client = translate.TranslationServiceClient()
    location = "global"
    parent = f"projects/{project_id}/locations/{location}"
    response = client.translate_text(
        request={
            "parent": parent,
            "contents": [text],
            "mime_type": "text/plain",  # mime types: text/plain, text/html
            "source_language_code": "en-US",  # 翻訳前の言語を指定
            "target_language_code": "ja",  # 翻訳後の言語を指定
        }
    )
    for translation in response.translations:
        print(response)
        # translations {
        #   translated_text: "\343\203\215\343\202\263"
        # }

        print(response.translations)
        # [translated_text: "\343\203\215\343\202\263"
        # ]

        print(translation)
        # translated_text: "\343\203\215\343\202\263"

        print(translation.translated_text)
        # ネコ
...
```


## 参照
* https://cloud.google.com/translate/docs/setup
* https://cloud.google.com/endpoints/docs/openapi/enable-api?hl=ja#gcloud
* https://cloud.google.com/translate/docs/advanced/translating-text-v3


## ちなみに
Googleスプレッドシートでの翻訳方法です。

[GoogleスプレッドシートでまとめてGoogle翻訳](https://zenn.dev/komiya/articles/e493c97c8591823ac58cｈ)