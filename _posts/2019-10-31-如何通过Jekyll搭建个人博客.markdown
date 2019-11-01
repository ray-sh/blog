---
layout: post
title:  "如何通过Jekyll搭建个人博客"
date:   2019-10-31 12:28:39 +0800
categories: jekyll update
---
## 在Mac上安装Jekyll遇到的问题
1. Ruby版本太低，解决方法是用brew install安装一个新的ruby，然后通过修改PATH来使用它
2. bundler版本过低，通过bundle update --bundler
3. Docker很方便，但是如果之前没有用过最好不要尝试，因为这又引入了一层更加复杂的技术栈，学习一门新的技术，一定是低层到高层的路线。

## 理解Jekyll的运行原理

Jekyll本质上是一个Ruby的程序，他可以把markdown类型的文本编译成html，加上一定的布局和css，就可以形成blog的website。它本身还包含一个
本地的local web server，用来实时查看编译生成的站点。对Ruby不太理解的新手还是不太容易理解Jekyll。

### Ruby背景知识介绍
要理解Jekyll，需要对Ruby application的构成有一定的了解。
#### Gem
1. Gem可以理解为java的jar，它是Ruby程序打包的方式，安装完ruby以后就有了/usr/local/Cellar/ruby/2.6.2/bin//gem
2. Gem也是一个命令行工具，用来下载，安装，打包 Gem package

```
gem --help
RubyGems is a sophisticated package manager for Ruby.  This is a
basic help message containing pointers to more information.

  Usage:
    gem -h/--help
    gem -v/--version
    gem command [arguments...] [options...]

  Examples:
    gem install rake
    gem list --local
    gem build package.gemspec
    gem help install

  Further information:
    http://guides.rubygems.org
```

#### Gemfile
A file which contains list of gems required for your site.

#### Bundler
[Bundler manages an application's dependencies through its entire life, across many machines, systematically and repeatably
](https://rubygems.org/gems/bundler)

## Jekyll目录结构

## Jekyll编译流程
## 定制个人的Blog站点

## 自动上传Blog到Github
