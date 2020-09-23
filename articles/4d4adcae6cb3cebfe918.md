---
title: "GoogleスプレッドシートでまとめてGoogle翻訳"
emoji: "📝"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["GSuite", "Google Spread Sheet"]
published: true
---

# やること

これを
![](https://storage.googleapis.com/zenn-user-upload/2uf63k8ussi2w1n79bvljdusyr11)

こうっ
![](https://storage.googleapis.com/zenn-user-upload/xcivcng8sxkce65lfir8p24ha6et)

# やり方
GOOGLETRANSLATE()関数を使います
※ excelにはない関数です。

## 1.

翻訳後の文字列を表示したいセルを選択して以下を入力

=GOOGLETRANSLATE(A1,"en","ja")
=GOOGLETRANSLATE(翻訳ターゲットのセル,"翻訳前の言語コード","翻訳後の言語コード")

![](https://storage.googleapis.com/zenn-user-upload/yrkn1g4k4sfhqn4l9dd6ieiqkbzw)

こうなる
![](https://storage.googleapis.com/zenn-user-upload/keitorwsyen3fho6er35glhljvvk)

## 2.

ネコのセルの右下をドラッグして引っ張ります
![](https://storage.googleapis.com/zenn-user-upload/4js7y57npsmwnmb1xar8jppc2e8a)

こうなる
![](https://storage.googleapis.com/zenn-user-upload/87sf2gxfkxs1zohu5znvseorgauw)

## ちなみに
pythonだとこんな感じです
⇣
[PythonでGoogle翻訳](https://zenn.dev/komiya/articles/e493c97c8591823ac58c)