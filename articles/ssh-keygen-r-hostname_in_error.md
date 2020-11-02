---
title: "sshでWARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!な時"
emoji: "👌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ssh, shellscript]
published: true
---

# こんなときは

```sh
ssh example.com

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
Please contact your system administrator.
Add correct host key in /Users/anata_no_user_name/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/anata_no_user_name/.ssh/known_hosts:26
ECDSA host key for xx.xx.xxx.xxx has changed and you have requested strict checking.
Host key verification failed.
```

※ クラウドサーバのインスタンスを作り直したりして外部IPが変わると、こうなったりする。

# こうっ

```sh
ssh-keygen -R example.com # <-HOSTNAME
```

~/.ssh/known_hostsから指定したホストの鍵を削除してくれる。
直接known_hostsファイルを編集して削除してもOK。
