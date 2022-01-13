---
layout: post
title: GPD micro PC
tags:
- umpc
- linux
- manjaro
- memorandum
---

![]({{ site.repository }}/assets/images/2021/07/27_gpd-micro-pc/image001.jpg)

ちょっと前から、外での打ち合わせやメモ、小作業用として、**中国・深センGPD Technology**の**GPD micro PC**を使っている。  
標準のインストールOSはWindows10だが、筆者は変わり者なので**Manjaro Linuxのi3エディション**をインストールして使っている。  
今回はその端末について書いてみる。  

<!--more-->

## ポメラの感動が忘れられない世代へ

KING JIMが発売していたポメラという端末がある。  
7インチと小型だがフルサイズのキーボードでメモが取れる画期的商品だ。  
だが、筆者は一点においてこの商品をうまく使えなかったのである。  

そう、この商品は**日本語キーボード配列+ATOK**で構成されているのだ。  

普段から*USキーボード配列+Google日本語入力*を活用する筆者には勝手が違う商品だ。

## 救世主かのように現れたGPD商品

そこで筆者が目を付けたのがGPDのUMPCである。  
過去にGPD Pocketを使っていた筆者にとっては変則キー配列は覚えればいいものであったし、何より英字配列と同じ感覚で使えるのはポイントが高かった。  

デフォルトのOSはWindowsだが、Linuxもインストールできるので不要なものを削ぎ落とした、自分好みな端末にカスタマイズも容易だ。  
幸いなことにArch Linux系の導入は多くの方が実践しており、参考にできる文献も豊富だった。  

以上の点から、GPDのUMPCはポメラのような小型端末を作る素材としてうってつけだった。  

## GPD Mirco PC

今回素材として選択したのは、GPDのエンジニア向けUMPC **GPD Micro PC**の2021モデルだ。  

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=syabu9190c-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B08YN3W2R9&linkId=2470356871fac3e55787b25050ae802a"></iframe>

スペックは下記のような感じである。  

- 6インチサイズ
- Windows 10 Pro 64bit
- Intel Celeron N4120 Processor
- メモリ8GB
- 256GB SSD
- USキーボード配列(変則)
- インターフェイス
  - 3×USB 3.0 Type A
  - 1×USB 3.0 Type C
  - 1×HDMI 2.0 Type A
  - 1×RS-232
  - 1×RJ45
  - 1×microSDカードスロット (SDXC対応、最大2TB)
  - 1×3.5mmオーディオIN/OUT
- バッテリー: 2×3,100mAh

大体満充電状態で4～5時間くらい使える。(ネットにフル接続していた場合  
モバイルバッテリーでも充電できるので、バッテリー併用で実質1日外で使うことが可能  

こちらの端末に到着早々、Manjaro Linuxをインストールしてセットアップしていく。  
※念の為Windows10のシリアルはメモしておく。  

## Manjaro Linuxのインストール

[Manjaro Linux公式](https://manjaro.org/)よりインストールメディアデータをダウンロードしてくる。  
私は*i3 エディション*を使っているが、お好みで別のウィンドウマネージャでも良い。  

USBを差し込んだら電源ボタンを投入、`esc`を押してブートメニューを呼び出し、USBから起動してインストールに進む。  

この際画面が縦になってしまう場合は下記のコマンドで横にすれば作業がしやすくなる。

```bash
# 画面回転コマンド
$ xrandr -o right
```

パーティションの設定などをこだわらなければインストールはすぐ済む。  
日本語で使う場合は言語などは極力日本語にしておくと後から設定する項目が少なくて済む。  
インストールが終わったら再起動してディスクにインストールしたManjaro Linuxが起動する。  

## インストール後の設定(最低限)

まずは起動ごとに画面が回転するのをなんとかしたい。  
`/etc/X11/xorg.conf.d/20-display.conf`のファイルを作成して再起動する。  

```/etc/X11/xorg.conf.d/20-display.conf
Section "Monitor"
  Identifier  "DSI-1"
  Option      "Rotate"  "right"
EndSection
```

- *2022.1.14修正: 内蔵ディスプレイの設定をIntelドライバからMesaドライバに変更*

また、pacman(パッケージ管理システム)のサーバも最適化させておこう。  
下記のコマンドを使えば自動で応答が速いサーバをセットアップしてくれる。  

```bash
# pacmanのサーバを自動最適化
$ sudo pacman-mirrors --geoip
```

ホームディレクトリの日本語化についてはこちら。  
設定後再起動すればホームディレクトリフォルダが英語になる。  

```bash
# インストールコマンド
$ sudo pacman -S xdg-user-dirs-gtk
# 実行コマンド
$ LANG=C xdg-user-dirs-gtk-update
```

あとは日本語入力環境も入っていないので、インストールする。  
フォントやIMEはお好みだが、これくらいが入っていれば良いだろう。  

```bash
# フォントインストール
$ sudo pacman -S noto-fonts-cjk noto-fonts-emoji noto-fonts-extra otf-ipafont otf-ipaexfont otf-ipamjfont
# Fcitxインストールコマンド
$ sudo pacman -S fcitx5-im fcitx5-mozc
```

`~/.xprofile`の作成もやっておく。  

```~/.xprofile
export LANG="ja_JP.UTF-8"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

そして再起動。  

以上で基本的なセットアップは終了。  
Manjaro LinuxはAURをインストールするためのyayコマンドなども標準で対応している。  
通常のGoogle Chromeなどもコマンドでインストールできるので、お好みの環境を手早く構築できることだろう。  

この記事がUMPCでお好みの環境を作りたいと考えている方の助力になれば幸いである。  
