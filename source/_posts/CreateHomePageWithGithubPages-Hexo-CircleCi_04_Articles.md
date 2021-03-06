---
title: Github-Pages-Hexo-CircleCiでホームページを作る_04_記事を書く編
date: 2018-06-30 22:37:19
tags:
- blog
- hexo
- CircleCI
---

タイトルの通り、ホームページを構築したので、最初の記事の練習と備忘録として記事を作成  

記事作成と画像の挿入編

* * *

## 記事を書く

### 各種設定

各種と言っても入れたいものは今の所は1つだけ  
記事を作成する際に、付随情報を格納する用フォルダが作成される

    post_asset_folder: true

今後、使って行きたい機能が増えたら追記していくかもしれない

### 記事の作成

記事の作成は以下のコマンドを実行し、作成されるMDファイルにマークダウン形式で記事を書くだけ  
また、上記の設定をしていると、付随する情報を格納するためのディレクトリが同時に作成される
```
hexo new <記事のタイトル>
```
その後`hexo generate`のコマンドを作成する事でその記事のHTMLが生成される

### 画像の挿入

Hexoを使用して記事に画像を挿入する方法として、一般的なマークダウン形式の記法も使用可能だが、  
今回は`post_asset_folder: true`の設定をいれてあるので、Hexoの独自記法を使用することにした。

前提として、`Hexo new`した時に精製される、 **記事のタイトルのディレクトリ** に画像を格納する
その上で以下のように記載する
```
{% asset_img [class names] slug [width] [height] [title text [alt text]]%}
```
詳細はこちら→[img](https://hexo.io/docs/tag-plugins.html#Image) / [asset_img](https://hexo.io/docs/tag-plugins.html#Include-Assets)

サンプル

-   MDサンプル
```
{% asset_img lena.jpg 150 "lena" %}
```
-   画像の表示  
    {% asset_img lena.jpg 150 "lena" %}
