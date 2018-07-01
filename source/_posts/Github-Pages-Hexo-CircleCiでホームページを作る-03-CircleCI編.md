---
title: Github-Pages-Hexo-CircleCiでホームページを作る_03_CircleCI編
date: 2018-06-29 00:59:28
tags: blog
---

タイトルの通り、ホームページを構築したので、最初の記事の練習と備忘録として記事を作成  

CiecleCI編

* * *

## CircleCIと連携する

masterブランチへマークダウンをコミットした際に、  
今までしてきたHexoのデプロイの設定～デプロイまでを自動でやってもらうように設定する


### CircleCIの設定のデフォルトを取得する



- 実際にやっていないので不明だが、こちらを先にやった方がいいかもしれない  
→ [SSHの設定](SSHの設定)


以下の手順で`circle.yml`の雛形を取得する  
-   プロジェクトを追加を選択<br>
{% asset_img CircleCISetupProject.PNG "CircleCISetupProject" %}

-   プロジェクトを追加をクリック<br>
{% asset_img CircleCIAddProject.PNG "CircleCIAddProject" %}

-   言語の設定はNodeを選択<br>
{% asset_img CircleCILanguage.PNG "CircleCILanguage" %}

-   `circle.yml`のデフォルトの設定が表示される<br>
{% asset_img CircleCIYmlDefault.PNG "CircleCIYmlDefault" %}

### Github側の設定

`circle.yml`を作成し、masterブランチへpushする
上記で取得したデフォルトと、[CircleCI 2.0 Docs](https://circleci.com/docs/2.0/)を参考に、  
`circle.yml`をこのように編集した

```
# Javascript Node CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-javascript/ for more details
#
version: 2
jobs:
  build:
    docker:
      # specify the version you desire here
      - image: circleci/node:8.11.3
        environment:
          TZ: /usr/share/zoneinfo/Asia/Tokyo
      # Specify service dependencies here if necessary
      # CircleCI maintains a library of pre-built images
      # documented at https://circleci.com/docs/2.0/circleci-images/
      # - image: circleci/mongo:3.4.4

    working_directory: ~/HomePage
    branches:
      only:
        - master
    steps:
      - run:
          name: update-npm
          command: 'sudo npm install -g npm@6'
      - checkout
      - run:
          name: install-npm
          command: npm install
      - run: git config --global user.name "KoinoEngineering with CircleCI"
      - run: git config --global user.email "email@koino.engineering"
      - run: git submodule init
      - run: git submodule update
      - run: ./node_modules/.bin/hexo clean
      - run: ./node_modules/.bin/hexo generate
      - run: ./node_modules/.bin/hexo deploy
```

設定の概要

- nodeのバージョンは8.11.3(準備編でインストールしたバージョン)
- 作業ディレクトリはリポジトリ名
- masterブランチへのpushにのみ反応する
- npmをアップデート（デフォルトのnpmが古かったため）
- run:コマンドに実行したい内容を１行づつ記載

## CircleCI側の設定

[CircleCIの設定のデフォルトを取得する](#CircleCIの設定のデフォルトを取得する)でやったところの続きから  
(戻ってしまっていた場合にはもう一度やればOK)  

-   Start buildingをクリック<br>  
　　　　{% asset_img CircleCIStartBuilding.PNG "CircleCIStartBuilding" %}

-   成功時はCircleCIのサイトの方で以下のようにログが出力されていくが、正しく設定できていても  
    初回は`hexo deploy`コマンドで**失敗する**  
    理由は下記
        ※deploy以外のコマンドで失敗した場合には設定が誤っているので、ログ等を確認して修正する  
        {% asset_img WorkCricleCI.PNG "WorkCricleCI" %}

-   <span id="SSHの設定">SSHの設定</span>  
    CircleCIが持っているSSHキーは読み取り権限しかもっていないため、pushできない  
    pushできるようにするために以下の設定を行う
    1.  Add User Keyをクリック  
    1.  githubとの連携の認証を聞かれるので承諾する
    1.  SSHキーが追加される  
　　    {% asset_img AddSshKey.PNG "AddSshKey" %}

## 自動デプロイ

ここまでの設定が正しくできていれば、masterブランチがpushされると  
CircleCIが自動でgh-pagesブランチへのデプロイを行うようになる

{% asset_img WorkCircleCI.PNG "WorkCircleCI" %}
