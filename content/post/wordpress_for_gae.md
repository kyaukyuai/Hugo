+++
title = "Google App Engine で始める WordPressでのサイト構築"
date = "2016-06-26T11:14:00+09:00"
description = ""
tags = ["wordpress", "google app engine"]
categories = ["operation"]
image = "wordpress_for_gae.jpeg"
+++

近頃、耳にする機会の増えている[GCP(Google Cloud Platform)](https://cloud.google.com/)、2016年後半には日本リージョンの開設も予定されている.
そのGCPのサービスの一つである[GAE(Google App Engine)](https://cloud.google.com/appengine/)を利用して、[WordPress](https://ja.wordpress.com/)でのサイト構築を試してみる.

## `GCP project` の作成

まずは、[GCPのダッシュボード](https://console.cloud.google.com/home/dashboard)よりプロジェクトを作成する.
Headerの`Create a project...`より作成する.
project名は任意で良い.

## `Cloud SQL` の作成

[ここ](https://cloud.google.com/sql/docs/create-instance)より、Cloud SQLのインスタンスを作成する.
Instance IDは任意でも良いが、`wordpress`にしておくと後々便利である.
ここでの注意点は2点.

### 1. Authorized IP AddressesにIPアドレスを追加

[Your IP Address](https://www.google.com/#q=whats+my+ip)に記述されているIP Adressを、Authorized networksに追加する.

### 2. Regionはus-xxxを選択

地域を「米国」にしないとApp Engineから接続できない.
自分は暫くここではまってしまった.

## Ref.

[公式: Quick Start WordPress for Google App Engine](https://googlecloudplatform.github.io/appengine-php-wordpress-starter-project/)
