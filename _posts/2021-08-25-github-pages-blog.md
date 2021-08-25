---
layout: post
title: Github Pagesでブログを作る。
tags:
- programing
- github
- jekyll
- memorandum
---

Github pagesを使ってブログを公開するのがエンジニアの流行りなのでやり方をザックリまとめる。  

<!--more-->

## そもGithub Pagesとは

Githubのリポジトリに静的サイトを公開できるホスティングサービスの一つ。  
本来はリポジトリのランディングページなどとして利用するもののハズだが、エンジニアのポートフォリオなどとして使われることも多い。  

内部でJekyllというRuby製の静的サイトジェネレータが動いている。  

## Jekyllとは

Ruby実装の静的サイトジェネレータ。  
Markdown文書でページや記事の記述ができるのが特徴。  
詳しくは[Jekyllの公式サイト](https://jekyllrb.com/)をチェック。  

今回Github Pagesでブログを開設するにあたり、このJekyllを活用する。  
なお、Github PagesのJekyllは少々制約もあるので注意。  
そのあたりに関しては[Githubのドキュメント](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll)を参照すること。  

## ブログのために必要なもの

必要なものは下記のものである。

- githubのアカウント
- 公開用リポジトリ(github.ioでも問題なし)
- アップロードするためのファイル一式

アカウントやリポジトリについては割愛する。  

## ブログ用のファイルを作成する

### 最低限ファイル構成

- /
  - /\_layout/
    - default.html
  - /\_posts/
    - 2021-08-25-post.md
  - /\_sass/
    - jekyll-theme-hacker.scss
  - /assets/
    - /css/
      - style.scss
  - \_config.yml
  - index.md

`jekyll-theme-hacker.scss`については使用する[テーマ](https://pages.github.com/themes/)によって変更する。  
上記がブログを公開する上での最低限のファイル構成となる。  

ページ内のパーツの分割などを行う場合は`_includes`などのフォルダや必要ファイルを作成する。  

### \_config.ymlの設定

```yaml
theme: jekyll-theme-hacker
```

`_config.yml`ファイルで最低限必要な設定はこれくらいとなる。  
他サイト名や執筆者などは必要であれば行う。  

### /\_layouts/default.htmlの作成

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset='utf-8'>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Blogs</title>
    <link rel="stylesheet" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
  </head>
  <body>
    <header>
      <h1>{{ page.title }}</h1>
    </header>
    <main>
      {{ content }}
    </main>
  </body>
</html>
```

1枚のレイアウトですべてのページに対応するのであれば必要なコードはこれくらいである。  
もし、`<title></title>`部分などを定数で管理する場合は`_config.yml`に記述する。  

### /\_sass/jekyll-theme-hacker.scssと/assets/css/style.scssの作成

`/_sass/jekyll-theme-hacker.scss`については従来のスタイルと同様の記述でOK。  
`/assets/css/style.scss`は`/_sass/jekyll-theme-hacker.scss`を読み込んでくるようにしたいので下記のように記述する。  

```scss
---
---

@import 'jekyll-theme-hacker';
```

### index.mdと/\_posts/2021-08-25-post.mdを作成

いずれも利用するレイアウトとタイトルを指定する。  
その際の記述は下記のようになる。  

```markdown
---
layout: front
title: blogs
---
```

`---`の続きからページの内容を記載していく。  
記事の一覧などを作成する必要がある場合は下記のようなコードを記載する。  

```html
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">
        <h2>{{ post.title }}</h2>
      </a>
    </li>
  {% endfor %}
</ul>
```

## pushして公開

ここまで作成できたら後はリポジトリにプッシュしてGithub Pagesを公開する設定をリポジトリに行えば完成である。  

ここまで手軽にMarkdown文書で記述できるブログが手に入るサービスもなかなかないので、コードを書く心得がある方はチャレンジしてみてもいいだろう。  
ページ内にはAmazonのリンクなども貼り付けできるのでちょっとしたお小遣い稼ぎにも使える。  
