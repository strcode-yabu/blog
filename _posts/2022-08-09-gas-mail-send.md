---
layout: post
title: 【GAS】メールフォームのバックエンドをGASで
tags:
  - google
  - gas
  - script
  - web
  - programming
---

Web の学習で HTML・CSS でサイト作成を行ったとき、誰しもお問い合わせフォームというものを作成するのではないだろうか。  
HTML・CSS を用いてみてくれまでは作れるかもしれないが、その先のお問い合わせの送信処理は PHP などを用いてサーバサイドのコードを書かないといけないので、かなり気合の入った人でないと作ることは難しい。

だが、その常識も今変わろうとしている。  
Google が提供している **Google Apps Script** を活用することにより PHP などが使えるサーバを用意することなく、送信処理を実装することが可能となる。  
早速見てみよう。

<!--more-->

## Google Apps Script とは

Google が提供する JavaScript をベースとしたスクリプト言語である。  
Google ドキュメントやスプレッドシートのマクロのような側面で使われていたが、Gmail やカレンダーを操作することもできるので様々なことが実行できる。  
利用には Google のアカウントが必要になる。  
※本記事では Google のアカウント作成については割愛する。

Google Apps Script について、詳しくは[こちら](https://developers.google.com/apps-script/)。

## 記事移動情報

※この記事は [Zenn](https://zenn.dev/s_yabu/articles/20240626-form-backend-on-gas) に移植しました。

