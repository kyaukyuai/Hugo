+++
author = "kyaukyuai"
categories = ["docker"]
date = "2016-07-24T21:42:34+09:00"
description = "Docker-nization"
featured = "docker-nization.jpeg"
featuredalt = "docker-nization"
featuredpath = "/images"
linktitle = ""
title = "Rails on Docker"
type = "post"

+++

# Getting Started with Docker for Mac

[Getting Started with Docker for Mac](https://docs.docker.com/docker-for-mac/) の手順に従って、Docker for Macをインストールする.

# Dockerfileの作成

```
touch Dockerfile
vim Dockerfile
cat Dockerfile
---
FROM ruby:2.2.3
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev postgres-client nodejs
RUN mkdir /app
WORKDIR /app
ADD Gemfile /app/Gemfile
ADD Gemfile.lock /app/Gemfile.lock
RUN bundle install
ADD . /app
---
```

# docker-composeによるオーケストレーション

```
touch docker-compose.yml
vim docker-compose.yml
cat docker-compose.yml
---
version: '2.0'
services:
  db:
    image: postgres:9.5.3
    expose:
      - "5432"
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    depends_on:
      - db
---
```

# コンテナ起動

```
docker-compose build
docker-compose run web bin/rake db:setup
docker-compose up
```

# 動作確認

http://localhost:3000

# コンテナ起動後の操作

| ローカル開発時 | Docker Compose |
|----------------------|:-------------------------------------------:|
| rails g model Member                     | docker-compose run web rails g model Member |
| rake db:migrate                          | docker-compose run web rake db:migrate      |
| rails dbconsole                          | docker-compose run web rails dbconsole      |
| RAILS_ENV=test bundle exec rake db:setup | docker-compose run -e RAILS_ENV=test web bundle exec rake db:setup |


# Webサーバのスケールアウト

```
docker-compose scale web=3
```