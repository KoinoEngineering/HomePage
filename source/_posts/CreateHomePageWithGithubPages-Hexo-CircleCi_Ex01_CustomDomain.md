---
title: Github-Pages-Hexo-CircleCiでホームページを作る-番外編-カスタムドメイン
date: 2018-07-01 09:44:46
tags: blog
---

タイトルの通り、ホームページを構築したので、最初の記事の練習と備忘録として記事を作成  

カスタムドメイン編
* * *

## ドメインを用意する
任意のドメイン取得サイトなどを使用して、ドメインを取得する  
今回使用したのはこちら  
→<https://navi.onamae.com/>

## DNSの設定
取得したドメインのDNSの設定を参照したいリポジトリのGithub Pagesのドメインに設定する。

お名前.comの場合、以下の手順で設定画面を開くことができる  
- [DNS]のタブを開き、設定したいサーバー(今回は共用サーバー)を選択<br>  
{% asset_img SelectServer.png "SelectServer" %}
- 今回使用するドメインを選択<br>  
{% asset_img SelectDomain.png "SelectDomain" %}
- サーバーの画面から「独自ドメイン設定」を押下<br>  
{% asset_img OriginalDomain.png "OriginalDomain" %}
- ドメイン設定の画面の、今回使用するドメインの「DNS設定」を押下<br>  
{% asset_img DNSMain.png "DNSMain" %}
- 任意のレコードタイプがAのレコードの「変更」を押下  
※ ftp,pop,smtp,imapはそれぞれ別の用途のものなので、今回はwwwを使用<br>  
{% asset_img SelectHost.png "SelectHost" %}
- TYPEをCNAMEに変更し、ホストを_githubuser_.github.ioに変更する<br>  
{% asset_img SetCname.png %}
- 自分のドメイン(今回は<http://www.koino.engineering>)にアクセスして確認する  
この段階ではGithub側の設定ができていないので、Githubの404エラー画面が表示される<br>s  
{% asset_img GithubNotFound.png %}

## Githubの設定  
- CircleCiとHexoの挙動の都合上以下の方法ではダメだった方法  
    ~~Githubの設定はSettingsの画面から簡単に行うことができるが、それだと直接リモートのgh-pagesブランチを更新してしまうため、別の方法で実施することにする。~~  
    - ~~CNAMEというファイルをリポジトリの直下に作成し、中に連携したいドメインをホスト込みで記載する~~  
    ```
    www.koino.engineering
    ```
    - ~~これをpushすれば設定完了~~  


- 正しい方法  
`circle.yml`に以下の記載を追加する
位置は`checkout`〜`hexo deploy`の間であれば問題ない
```
 - run: echo "www.koino.engineering" > CNAME
```

## `_config.yml`の設定

カスタムドメインの設定をするとURLがサブフォルダで無くなるため、`_config.yml`の以下の設定を戻す  
```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com/
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```