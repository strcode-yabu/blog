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

※この記事は [Zenn](https://zenn.dev/s_yabu/articles/20240626-form-backend-on-gas) に移植しました。

<!--more-->

Web の学習で HTML・CSS でサイト作成を行ったとき、誰しもお問い合わせフォームというものを作成するのではないだろうか。  
HTML・CSS を用いてみてくれまでは作れるかもしれないが、その先のお問い合わせの送信処理は PHP などを用いてサーバサイドのコードを書かないといけないので、かなり気合の入った人でないと作ることは難しい。

だが、その常識も今変わろうとしている。  
Google が提供している **Google Apps Script** を活用することにより PHP などが使えるサーバを用意することなく、送信処理を実装することが可能となる。  
早速見てみよう。

## Google Apps Script とは

Google が提供する JavaScript をベースとしたスクリプト言語である。  
Google ドキュメントやスプレッドシートのマクロのような側面で使われていたが、Gmail やカレンダーを操作することもできるので様々なことが実行できる。  
利用には Google のアカウントが必要になる。  
※本記事では Google のアカウント作成については割愛する。

Google Apps Script について、詳しくは[こちら](https://developers.google.com/apps-script/)。

## 実装の流れについて

### 新規のプロジェクトを作成する

![""]({{ site.repository }}/assets/images/2022/08/09_gas-mail-send/image001.png)

Google ドライブにアクセスし、新規作成を行う。

![""]({{ site.repository }}/assets/images/2022/08/09_gas-mail-send/image002.png)

作成するファイルには **Google Apps Script**を選択する。

### プロジェクト内に`doPost`関数を作成する

```javascript
const doPost = (e) => {
  Logger.log(e);
};
```

上記のようなコードを書くことによりフォームから送られてきた`POST`データを受け取ることができる。
`e.parameter`にフォームのデータが含まれているので、そちらからデータを取得する。  
例えば`name(送信者名前)`, `email(送信者メールアドレス)`, `content(送信内容)`を受け取った場合のコードはこのようになる。

```javascript
const doPost = (e) => {
  Logger.log(e);
  const name = e.parameter.name;
  const email = e.parameter.email;
  const content = e.parameter.content;
};
```

その他の詳しいパラメータなど知りたい方は[公式ドキュメント](https://developers.google.com/apps-script/guides/web)を。

### 送信用の関数を作成する

POST データを受信できたので、次は Gmail を使ってメールを送信する処理を書く。  
メールの送信には`GmailApp.sendEmail()`を使用する。

```javascript
const sendMail = (name, email, content) => {
  const recipient = email;
  const recipientName = name;
  const subject = "Test Mail Title";

  const body =
    recipientName + "様より\n" + "\n" + "テストメールです。\n" + "\n" + content;

  const options = {
    from: "test@example.com",
    name: "テスト太郎",
  };

  GmailApp.sendEmail(recipient, subject, body, options);
};
```

と、このような感じである。
今回オプションには`from`と`name`を設定しているが、`cc`や`bcc`なども設定可能である。  
詳しいオプションについては[公式ドキュメント](https://developers.google.com/apps-script/reference/gmail/gmail-app)を参照。

送信用の関数ができたらそちらに値を渡せるように`doPost`関数を修正する。

```javascript
const doPost = (e) => {
  Logger.log(e);
  const name = e.parameter.name;
  const email = e.parameter.email;
  const content = e.parameter.content;

  sendMail(name, email, content);
};
```

### 処理が完成したら Web アプリとして公開する

![""]({{ site.repository }}/assets/images/2022/08/09_gas-mail-send/image003.png)

右上の **デプロイ** ボタンをクリック、 **新しいデプロイ** を実施する。

![""]({{ site.repository }}/assets/images/2022/08/09_gas-mail-send/image004.png)

新しいデプロイ画面が出てきたら **種類の選択** のハグルママークをクリップしてウェブアプリを選択する。  
特に設定をいじる必要はないが、わかりやすいように説明の部分は入れておくと管理しやすくできる。

デプロイが終わったらウェブアプリ用の URL が発行されるので、そちらを`<form></form>`タグの`action`属性の値に設定する。  
※このさいに`method`属性が`post`になっていることも確認しておく。

### 動作テスト

ここまで出来たら送信できるか確認してみる。  
メールを受信できたら OK である。

ちなみに今回はサンクス画面への遷移などは設定してないので、実際に活用するときには必要に応じてそちらも設定すること。

## 最後に

どうだろうか。  
ざっくりと見てみたがかなり簡単にフォームのバックエンド処理が実装できた。  
PHP などサーバ側はまだわからないけど、実際に使えるフォームが作りたい場合はひとつの選択として GAS を選んでみるのも良いだろう。

## 参考サイト

- [GAS で web アプリの作成とパラメータの確認方法(doGet、doPost)](https://breezegroup.co.jp/201906/gas-get/)
- [GAS で GET,POST を受け取る方法](https://kin29.info/gas%E3%81%A7getpost%E3%82%92%E5%8F%97%E3%81%91%E5%8F%96%E3%82%8B%E6%96%B9%E6%B3%95/)
- [【GAS】Gmail でメールを送信するには？](https://masagoroku.com/%E3%80%90gas%E3%80%91gmail%E3%81%A7%E3%83%A1%E3%83%BC%E3%83%AB%E3%82%92%E9%80%81%E4%BF%A1%E3%81%99%E3%82%8B%E3%81%AB%E3%81%AF%EF%BC%9F)
