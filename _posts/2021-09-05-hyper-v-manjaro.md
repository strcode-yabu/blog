---
layout: post
title: Manjaro Linux in Hyper-V
tags:
- windows
- hyperv
- linux
- manjaro
- memorandum
---

![]({{ site.repository }}/assets/images/2021/09/05_hyper-v-manjaro/image001.jpg)

Windows10のProエディションにインストールされている**Hyper-V**を使ってManjaro Linuxの仮想マシンを動かしてみたので、備忘録をまとめる。

<!--more-->

## Hyper-Vとは

ざっくりいうとMicrosoft製のVMWare、VirtualBOX。  
クライアント版はWindows8から搭載され今日に至る。  

基本Pro, Education, Pro Education, EnterpriseなどHome以上のエディションに提供されている。  

機能をコマンドラインなどに大幅制限した無償版のHyper-V Serverも提供されている。  

- [Hyper-V - Wikipedia](https://ja.wikipedia.org/wiki/Hyper-V)

## Hyper-Vを使うには

Hyper-Vはデフォルトでは機能が無効化されており、**プログラムと機能 > Windowsの機能の有効化または無効化**から有効にする必要がある。  

![]({{ site.repository }}/assets/images/2021/09/05_hyper-v-manjaro/image002.png)

こちら有効にして再起動をするとスタートメニューの中の**Windows 管理ツール**の中に**Hyper-V クイック作成**、**Hyper-V マネージャ**が追加されている。  

## インストールイメージの用意

今回はManjaro Linuxをインストールする。  
インストールする`.iso`イメージは[こちら](https://manjaro.org/)のサイトよりお好みのエディションを入手する。  

## 仮想マシンを作成する

**Hyper-V マネージャ**から**操作 > 新規 > 仮想マシン**で新規の仮想マシンを作成する。  
仮想マシンのスペックは今回は下記で作成した。  

- 仮想マシンの世代
  - 第2世代

- 起動メモリ
  - 2048 MB (ホストPCの1/8)
  - 動的メモリを確保する
- 仮想プロセッサの数
  - 4 （ホストPCの1/2）
- ネットワークアダプタ
  - Default Switch
- セキュアブート
  - 無効

いくつかは設定ウィザード画面上には出てこないので、仮想マシン作成後に仮想マシンの**設定**から設定する。  
また、DVDドライブにダウンロードしてきた.isoイメージを設定し忘れないように。
設定が終わったら仮想マシンに接続し起動する。

## インストールについて

インストールについては従来のPCにインストールするときとほぼ変わらないので割愛する。  

## キャレットが点滅したままになる場合の対処

起動してからキャレットが点滅したまま先に進まない現象が起こる。  
この現象が起きた場合はHyper-V用のビデオドライバを別途インストールする必要がある。  

対応としては下記を行う。

- `ctrl` + `alt` + `F2`を押してコンソールに入り、ユーザログインする。
- 下記のコマンドでインストール作業を行う
  - `$ sudo pacman -Sy`
  - `$ sudo pacman -S xf86-video-fbdev`
- インストールが終わったらログインユーザインタフェースである**LightDM**を起動する。
  - `$ sudo systemctl start lightdm`

筆者の環境では、インストール時と初回起動時の2回作業を行う必要があった。  

## おまけ: ウィンドウのサイズをワイドにする

Hyper-Vの仮想マシンで起動したPCはなぜか4:3の画面比になる。  
この場合は`/etc/default/grub`を編集し`GRUB_CMDLINE_LINUX`に`video=hyperv_fb:wwww×hhhh`を追記する。  
※wwww=画面幅 / hhhh=画面高さ  

```/etc/default/grub
# 例: 筆者の設定
GRUB_CMDLINE_LINUX="video=hyperv_fb:1280×720"
```

## 最後に

以上がHyper-VにManjaro Linuxをインストール際に特につまづきやすいポイントである。  
Hyper-Vを使ってManjaro Linuxを試そうとしている人でインストールにつまずいている人たちの助けになれば幸いである。  

## 参考記事

- [Hyper-VにManjaro Linuxをインストールするのが大変だったのでメモ](https://tkzwhr.hatenablog.com/entry/2019/09/14/120000)
- [Hyper-v で 仮想マシンの解像度の変更方法](https://qiita.com/mizutoki79/items/6122455bfa2755942488)
