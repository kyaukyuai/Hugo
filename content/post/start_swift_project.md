+++
title = "新規Swiftプロジェクト作成時に最初にやること"
date = "2016-07-17T23:43:42+09:00"
description = ""
tags = ["ios", "swift", "lint", "git", "pod"]
categories = ["operation"]
image = "start_swift_project.jpg"
+++

(最終更新日時: 2016/07/18)

この記事は随時更新予定です.

iOSアプリ作成のためにSwiftプロジェクトを作成する際に最初にやるべきことを記述していく.
いつもどうしてたか忘れてしまうので備忘録として記述する.


# .gitignoreテンプレートの作成

まずはバージョン管理の際に必要となる.gitignoreテンプレートの作成方法を記述する.
自分の場合は、[gemignore](https://rubygems.org/gems/gemignore)を利用している.

## gemignoreのインストール

```
gem install gemignore
```

## テンプレート生成

```
cd project_root
touch .gitignore
gemignore add swift
```

# CocoaPod

次に、外部ライブラリ利用時に不可欠な[CocoaPods](https://cocoapods.org/)の導入手順を記述する.

## cocoapods gemのインストール

```
gem install cocoapods
```

## Podfileの生成

```
pod init
```

## ライブラリをインストールor更新

```
pod install
or
pod update
```

# SwiftLint

次に、コード品質に不可欠なLintの導入手順を記述する.
ここでは、[SwiftLint](https://github.com/realm/SwiftLint)を利用する.

## swiftlintのインストール

```
brew install swiftlint
```

## Xcodeでのセットアップ

"Run Script Phase"に下記スクリプトを追加する.

```
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi

```

## .swiftlint.ymlの生成

```
touch .swiftlint.yml
cat .swiftlint.yml
```

```yml:.swiftlint.yml
line_length:
  - 200 #warning
  - 300 #error

file_length:
  - 600  #warning
  - 1200 #error
```

# Fabric

Fabricの指示に従って進めると特につまづくところはない.

# Reference

- [github:SwiftLint](https://github.com/realm/SwiftLint)

- [github:gemignore](https://github.com/x3ro/gemignore)