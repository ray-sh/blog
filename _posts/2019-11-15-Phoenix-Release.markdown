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
#执行webpack脚步
npm run deploy --prefix ./assets
#站点cache
mix phx.digest

rm -rf "_build"

MIX_ENV=prod mix release
```
## 如何部署phoenix application到PASS平台
最好的方法是dockerlize，这样能构方便的在不同平台迁移，并且很好的规避不同平台的部署差异，缺点是要对docker有一定的了解
