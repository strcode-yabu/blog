---
layout: post
title: Manjaro Linuxでサーバをたてる。
tags:
- linux
- manjaro
- server
- memorandum
---

使っていないノートPCにManjaro Linuxをインストールしてテスト用のサーバをたてたのでその備忘録。  
※OSインストールなどの過程はざっくり省くのでそのあたりは独力で。  

<!--more-->

## Webサーバのインストール

まずはWebサーバアプリをインストールする。  
今回は[Apache http server](https://httpd.apache.org/)を使う。  

```bash
sudo pacman -S apache
```

インストールが終わったら`/etc/httpd/conf/httpd.conf`をエディタで開き編集する。  

```
# 下記の行を見つけてコメントアウト
# LoadModule unique_id_module modules/mod_unique_id.so
```

ここまでできたらWebサーバの再起動を行う。  

```bash
sudo systemctl restart httpd
```

端末起動と同時にWebサーバをスタートさせる場合は下記のコマンドを使う。  

```bash
sudo systemctl enable httpd
```

Webサーバが動作しているか確認する場合は下記のコマンドを使う。  

```bash
sudo systemctl status httpd
```


## PHPのインストール

Apacheのインストールが終わったら、次は[PHP](https://www.php.net/)をインストールする。  

```bash
sudo pacman -S php php-apache
```

インストールが終わったら`/etc/httpd/conf/httpd.conf`をエディタで開き編集する。  

```
# 下記の行を見つけてコメントアウト
# LoadModule mpm_event_module modules/mod_mpm_event.so

# 下記の行を追記
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so

# PHP8であれば下記を追記
LoadModule php_module modules/libphp.so
AddHandler php-script php
Include conf/extra/php_module.conf
```

ここまでできたらWebサーバの再起動を行う。  

```bash
sudo systemctl restart httpd
```

## FTPの設定

Webサーバをたて終わったら、次はFTPソフトでファイルを転送できるようにする。  
今回は[vsftpd](https://security.appspot.com/vsftpd.html)を使う。  

まずは下記のコマンドでvsftpdをインストールする。  

```bash
sudo pacman -S vsftpd
```

インストールが終わったら`/etc/vsftpd.conf`をエディターで開き編集する。  

```
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
```
設定が完了したらFTP用の一意のユーザを作成する。  

```bash
sudo useradd -g ftp -d /srv/http -s /sbin/nologin ftpUserName
sudo passwd ftpUserName
```

FTPユーザを作成したら`/etc/vsftpd.conf`をエディターで開き編集する。  

```
nopriv_user=ftpUserNamme
```

ここまで設定できたらvsftpdを再起動する。  

```bash
sudo systemctl restart vsftpd
```

### 各種エラー対応

#### vsftpd 530 login incorrect

`/etc/pam.d/vsftpd`をエディターで開き変更する。  

```
# auth requiredpam_shells.so
# 上記の行を下記に変更する。
auth requiredpam_nologin.so
```

設定できたらvsftpdを再起動する。  

```bash
sudo systemctl restart vsftpd
```

#### 500 OOPS: cannot change directory

`/srv/http/`の権限を変更。  

```bash
sudo chmod 777 /srv/http/
sudo chown -R root.root /srv/http/
```


## 今後の追記予定

- mySqlのインストールから設定まで
- phpMyAdominの導入

## 参考など

- [[HowTo] Install Apache , MariaDB(Mysql),PHP (LAMP)](https://forum.manjaro.org/t/howto-install-apache-mariadb-mysql-php-lamp/13000)
- [manjaroインストールftpサーバー](https://ja.visual-foxpro-programmer.com/manjaro-install-ftp-server)