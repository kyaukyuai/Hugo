+++
author = "kyaukyuai"
categories = ["bluemix", "hubot"]
date = "2016-07-09T08:48:29+09:00"
description = "流行りのBluemix, 流行りのBot"
featured = "bluemix_bot2.jpeg"
featuredalt = "bluemix_bot"
featuredpath = "/images"
linktitle = ""
title = "Bluemixで始める無料bot運用 - Hubot編 -"
type = "post"

+++

[Bluemix](https://console.au-syd.bluemix.net/)を利用して24時間稼働のbotを無料で運用したいということで実施しました.
Bluemixの価格表は[こちら](https://console.ng.bluemix.net/pricing/)になりますので参考にしてください.
実際はTwiiterのボットを作成したので後ほど触れようと思います.

# 前提

前提として下記アカウントを既に保持しているとする.

- Bluemixアカウント

# hubotインスタンスの生成

[node.jsとnpm](https://docs.npmjs.com/getting-started/installing-node)を導入後、下記をコマンドによりhubot generatorをインストールする.

```
%  npm install -g yo generator-hubot
```

下記コマンドが正常に終了すると、[yeoman]()のhubot generatorが利用できるようになるので、新しくhubotのインスタンスを生成してみよう.ここでは、`ourhubot`という名前のインスタンスを生成する例を示す.

```
% mkdir ourhubot
% cd ourhubot
% yo hubot
```

ここで、gitを利用している場合は上記コマンドで生成されたファイルで初期化しよう.

```
% git init
% git add .
% git commit -m "Initial commit"
```

一旦`ourhubot`の完成で、ローカルマシンで動かすことが可能である.hubotはデフォルトでredisを必要としているため、動かす際にredisをインストールする、または`external-scripts.json`から`hubot-redis-brain`を除く必要があるので注意を.

```
% bin/hubot --name ourhubot
ourhubot>
```

# Bluemixへのhubotデプロイ

Bluemixのアカウントを既に保持している場合は、あとは必要となるのは[Cloud Foundry CLI](https://github.com/cloudfoundry/cli/releases)だけである.
macの場合は、下記コマンドでインストール可能である.

```
% brew tap cloudfoundry/tap
% brew install cf-cli
```

次に`manifest.yml`を定義します.そして`procfile`は不要になるので削除します.下記はSlack利用の場合の例になります.
詳しくは[こちら](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)を参考にして下さい.

```
applications:
- buildpack: https://github.com/jthomas/nodejs-v4-buildpack.git
  command: ./bin/hubot --adapter slack
  path: .
  instances: 1
  memory: 256M
```

`manifest.yml`の準備が終わりましたら、最終段階です.
下記コマンドではログイン情報が求められます.

```
% cd ourhubot
% cf api https://api.ng.bluemix.net
% cf login
```

いよいよ、Bluemixによる無料botの運用の開始です.
下記コマンドで稼働させてください.

```
% cf push NAME_OF_YOUR_HUBOT_APP
```

また、herokuのgithubコネクトと同様にBluemix側にgitのレポジトリを指定すれば、githubにpushからの自動アップデートが実現できます.

# Tips


### Bluemix CLI

`Clound Foundry`の`cf`コマンドに代わる`bleumix Cli`が登場している.

[こちら](http://clis.ng.bluemix.net/ui/home.html)からダウンロード可能です.

### Organizationの設定に注意

Organizationのロケーションとして、2016年7月9日現在で3箇所(シドニー、英国、米国南部)選択可能であるが、どこを選ぶかによってAPIのEndpointで指定するURLを変わってくるので注意を.

# 参考

- [Bluemix CLI](http://clis.ng.bluemix.net/ui/home.html)

- [github:hubot](https://github.com/github/hubot/blob/master/docs/index.md)

- [Bluemix価格表](https://console.ng.bluemix.net/pricing/)

- [Cloud Foundry Document:Deploying with Application Manifests](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)
