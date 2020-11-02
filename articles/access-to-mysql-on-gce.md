---
title: "GCE上のMySQL Serverにローカルからアクセスする"
emoji: "😽"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: true
---

# 前提
GCE上でMySQLコンテナが起動済み

# GCEインスタンスにタグを付与

```sh
$ gcloud compute instances add-tags db(インスタンス名) --tags mysqlserver(タグ名) --zone asia-northeast1-b
```

# ファイアウォールルールの作成

```sh
$ gcloud compute firewall-rules create mysql-remote-access(ファイアウォールルール名) \
--allow tcp:3306 \
--source-ranges xxx.xxx.xx.xx \ # 接続許可するIPを指定できる(指定しない場合0.0.0.0/0となる)
--target-tags mysqlserver(接続先インスタンスに付与したネットワークタグ名)
```


たとえば自宅のグローバルIPを--source-ranges で指定すれば、自宅から以外の接続を遮断できる。

## グローバルIP確認サイト
グローバルIP確認できます。
https://www.cman.jp/network/support/go_access.cgi


# MySQL接続確認
```sh
$ mysql -u hoge -pfuga -h インスタンスの外部IP -P 3306 --protocol=tcp
```

ちなみにローカルでコンテナをたてている場合は外部IPの部分をlocalhostにすればOK

```sh
$ mysql -u hoge -pfuga -h localhost --protocol=tcp
```