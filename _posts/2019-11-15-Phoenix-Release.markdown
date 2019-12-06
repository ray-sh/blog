---
layout: post
title:  "Deploy Phoenix"
date:   2019-11-15 12:28:39 +0800
categories: phoenix
---

## 如何制作phoenix release
制作phoenix release非常简单，只要按照 https://hexdocs.pm/phoenix/releases.html#goals 就可以

关键是要在assemble的时候，不需要提供SECRET_KEY_BASE，DATABASE_URL，这些值应该是runtime提供

- prod.secret.exs 从命名 releases.exs
- 修改releases.exs use Mix.Config -> import Config
- 修派prod.exs，#import_config prod.secret.exs
  
文档中的步骤可以用下面的脚步代替

```
#!/usr/bin/env bash
mix deps.get --only prod
MIX_ENV=prod mix compile
npm install --prefix ./assets
#执行定义在assets/package.json里面的deploy命令
#"scripts": {
#    "deploy": "webpack --mode #production",
#    "watch": "webpack --mode #development --watch"
#  },
#
npm run deploy --prefix ./assets
#站点cache
mix phx.digest

rm -rf "_build"

MIX_ENV=prod mix release
```

## 如何部署phoenix application到PASS平台
最好的方法是dockerlize，这样能构方便的在不同平台迁移，并且很好的规避不同平台的部署差异，缺点是要对docker有一定的了解

> The Dockerfile

```
FROM elixir:1.9.0-alpine as build

# install build dependencies
RUN apk add --update git build-base nodejs npm yarn python
# prepare build dir
RUN mkdir /app
WORKDIR /app

# install hex + rebar
RUN mix local.hex --force && \
    mix local.rebar --force

# set build ENV
ENV MIX_ENV=prod

# install mix dependencies
COPY mix.exs mix.lock ./
COPY config config
RUN mix deps.get
RUN mix deps.compile

# build assets
COPY assets assets
RUN cd assets && npm install && npm run deploy
RUN mix phx.digest

# build project
COPY priv priv
COPY lib lib
RUN mix compile

# build release
RUN mix release

# prepare release image
FROM alpine:3.9 AS app
RUN apk add --update bash openssl

RUN mkdir /app
WORKDIR /app

COPY --from=build /app/_build/prod/rel/yolo ./
RUN chown -R nobody: /app
USER nobody

ENV HOME=/app
```

Build docker image的过程遇到的问题
- npm not found, 解决方法：安装nodejs的同时安装npm
- 如果build景象的时候没有指定name，会生成重复的镜像，解决的方法 docker build -t yolo:v1 . (repo/tag)
- 国内镜像的加速？
- 每次镜像的编译都会从头来一便，即使一个很小的源代码的改动，如何解决？
  - docker image的构建是有cache，每次run都会生成一个新的层
- 镜像编译成功，但是web应用程序去没有启动，如何调试？应用程序的log放在哪里？
- 为什么会有两个from?
  - 第一个是构建build docker，如果已经有了本地的运行环境，可以跳过，但是如果要从源代码开始构建，最好是把build iamge准备好
  - 第二个是构建运行docker
  - 能否直接拷贝_build到一个linux镜像？
    - 可以使用bitwalker的elixir/phoenix镜像