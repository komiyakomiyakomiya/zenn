---
title: "Docker+nginxでGCEに静的ページをデプロイする"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [GCP GCE Docker nginx]
published: false
---

# SDKのインストール

# プロジェクト作成

# インスタンス作成 ~ Docker環境構築

# ディレクトリ構成
.
├── my_site
│   ├── index.html
│   ├── js
│   │    └── script.js
│   └── css
│        └── style.css
└── docker
    ├── Dockerfile
    └── server.conf


# サーバ設定ファイル

```conf:docker/server.conf
server {
  listen 80;
  server_name xx.xxx.xx.xxx; # <- インスタンスの外部IP

  server_tokens off;

  access_log /root/logs/access.log;
  error_log /root/logs/error.log;

  location / {
    root /root/public;
  }
}
```


# Dockerfile

```Dockerfile:docker/Dockerfile
FROM nginx

RUN mkdir /root/logs

RUN chmod 755 -R /root

COPY ./server.conf /etc/nginx/conf.d/default.conf

CMD ["nginx", "-g", "daemon off;"]
```

# ローカルファイルをGCEへ転送

```sh
$ scp -r -i ~/.ssh/path/to/id_rsa ./website GCPログインユーザ名@インスタンス外部IP(or ~/.ssh/configで設定したHostの文字列):/home/<ログインユーザ>/website
$ scp -r -i ~/.ssh/path/to/id_rsa ./docker GCPログインユーザ名@インスタンス外部IP(or ~/.ssh/configで設定したHostの文字列):/home/<ログインユーザ>/docker
```

転送先ディレクトリがない場合は自動で作成される


# dockerイメージビルド

```sh
$ docker build -t simple_nginx .
```

# コンテナ起動

```sh
$ docker run -it -p 80:80 --name webui_demo --mount type=bind,src=/home/ログインユーザーディレクトリ/__web_ui,dst=/root/public simple_nginx
```

# 権限変更
cssやjsなど一部の静的ファイルで403エラー。
ファイルのアクセス権限変更で解決。
```
$ chmod 755 ./Dropzone.js_files
```