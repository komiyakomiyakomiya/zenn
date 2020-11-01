---
title: "複数のGitHubアカウントでそれぞれのSSH Keyを分けて使いたい。"
emoji: "📝"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [git github]
published: true
---

# 前提
2つのGitHubアカウントがあります。
例えば、プライベート用と仕事用を使い分けるときなどでしょうか、

  * https://github.com/my-private-repo
  * https://github.com/my-job-repo

別々の公開鍵をそれぞれのGitHubアカウントに登録済み。

# ssh keyの保管場所
リポジトリ毎にディレクトリを分けてssh keyを保管。

* ~/.ssh/github_private/id_rsa
* ~/.ssh/github_job/id_rsa

# configファイルで設定

configファイルはこんな感じ。

```config:~/.ssh/config
...
Host github_private
    User git
    HostName github.com
    IdentityFile ~/.ssh/github_private/id_rsa

Host github_job
    User git
    HostName github.com
    IdentityFile ~/.ssh/github_job/id_rsa
...
```

# git remote set-urlでホストを設定

こんな感じでリポジトリが登録されていると思います。

```sh
git remote -v
origin  git@github.com:アカウント名/リポジトリ名.git (fetch)
```

こんな感じでホスト(@以下の文字列)をconfigファイルで設定しているHostにしてやればOK

```sh
git remote set-url origin git@github_job:アカウント名/リポジトリ名.git (fetch)
```

⇣

```sh
git remote -v
origin  git@github_job:アカウント名/リポジトリ名.git (fetch)
```


# こんなエラーが出た時に

私はconfigファイルを整理していたら、こんな権限エラーを吐くようになり、上記の方法で解消しました。
仕事用アカウントにpushしたいのに、プライベート用のssh keyを参照していた様子。

```sh
$ git push origin master
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```