+++
author = "kyaukyuai"
categories = ["hugo", "github", "wercker", "disqus", "googole analytics"]
date = "2016-06-22T22:20:21+09:00"
description = "Let's start your blog."
featured = "hello.jpeg"
featuredalt = "hello"
featuredpath = "/images"
linktitle = ""
title = "HugoとGitHubPagesで始める無料でのブログ運営 + Werckerによる記事公開の自動化"
type = "post"

+++

[モダン静的HTMLジェネレータHugo](http://gohugo.io/)と[GitHubPages](https://pages.github.com/)を利用して無料でブログを開設するための手続きを書き記す.
また、CI-as-a-Serviceとして有名な[Wercker](http://wercker.com/)を利用して記事公開までの流れを自動化する.

自分の動機・モチベーションとしては、普段ちょっと勉強したりしたことを忘れてしまうことが多いので、少しでも備忘録として残していくことである.

## Hugoのインストール

まずは、Hugoのインストールを行う.
macの場合は下記コマンドでインストール可能.

公式のインストール手順は[ココ](https://gohugo.io/overview/installing/).

```
brew install hugo
hugo version
// it should be as follows.
Hugo Static Site Generator v0.16 BuildDate: 2016-06-06T21:37:59+09:00
```

## Hugo作業用ディレクトリの作成

下記コマンドでサイト作成の作業用ディレクトリが作成できる.
ここでの作業用ディレクトリ名と実際のサイト名は関係ないので、Hugo作業用ディレクトリということが認識できる名前であれば問題ない.

```
hugo new site #{site_name}
cd #{site_name}
ls -lh
// command result shoule be as follows.
total 8
drwxr-xr-x  2 kyaukyuai  staff   68  6 22 22:17 archetypes
-rw-r--r--  1 kyaukyuai  staff  199  6 22 22:57 config.toml
drwxr-xr-x  3 kyaukyuai  staff  102  6 22 22:20 content
drwxr-xr-x  2 kyaukyuai  staff   68  6 22 22:17 data
drwxr-xr-x  2 kyaukyuai  staff   68  6 22 22:17 layouts
drwxr-xr-x  3 kyaukyuai  staff  102  6 22 22:55 static
drwxr-xr-x  3 kyaukyuai  staff  102  6 22 22:18 themes
```

## テーマのインストール

[公式テーマ一覧](http://themes.gohugo.io/)よりお気に入りのテーマを探し、インストールする.
ここでは、[hugo_theme_robust](http://themes.gohugo.io/robust/)を選択した例を示す.

```
cd themes
git clone https://github.com/dim0627/hugo_theme_robust.git
```

## 記事の作成

下記コマンドで記事のテンプレートを作成し、テンプレート(`hello.md`)を編集することで記事を作成できる.

```
hugo new post/hello.md
ls -l content/post/hello.md
cat content/post/hello.md

// command result should be as follows.
+++
date = "2016-07-03T17:19:38+09:00"
draft = true
title = "hello"

+++
```

## 記事の確認

```
hugo server -w -D -t hugo_theme_robust
```

wオプションを有効にすることで、記事が編集するとオートリロードされる.

Dオプションを有効にすることで、下書き中(draft=true)の記事も表示されるようになる.


## 公開用記事の作成

```
hugo
ls -l public
cd public
git init
echo ".DS_Store" >> .gitignore
git add -A
git commit -m "First Article"
git remote add origin https://github.com/kyaukyuai/kyaukyuai.github.io.git
git push -u origin master
```

[http://kyaukyuai.github.io](http://kyaukyuai.github.io)で公開記事を確認しよう.

## Tips

### `default.md`の作成によるデフォルトテンプレートの編集

`archetypes/default.md`を作成することで、デフォルトテンプレートを編集できる.

自分の場合は、下記のように編集している.

```
+++
title = ""
description = ""
tags = [ "", "" ]
date = "now()"
categories = [ "", "", ]

image = "" # optional
toc = false # optional, When set to FALSE this parameter, table of contents not appears in only this article.
+++
```

## Werckerによる記事公開の自動化

基本的には[公式の記事](http://gohugo.io/tutorials/automated-deployments)通りに進めることで実現できる.

下記に、`wercker.yml`を示す.

```ruby:wercker.yml
# This references a standard debian container from the
# Docker Hub https://registry.hub.docker.com/_/debian/
# Read more about containers on our dev center
# http://devcenter.wercker.com/docs/containers/index.html
box: debian
build:
  steps:
    - arjen/hugo-build:
        theme: hugo_theme_robust
        version: "0.15"
        flags: --buildDrafts=true

deploy:
  steps:
    - install-packages:
        packages: git ssh-client
    - lukevivier/gh-pages@0.2.1:
        token: $GIT_TOKEN
        repo: kyaukyuai/kyaukyuai.github.io
        basedir: public
```

唯一ハマったポイントがwerckerのworkflowにdeployを下記のように定義しなければならないことだ.

![wercker](/images/wercker.png)

## Ref.

+ [TANKSUZUKI.COM:Hugo + GitHub Pagesでブログを作る#1](http://tanksuzuki.com/post/hugo-github-pages-1/)
