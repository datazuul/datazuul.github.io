# datazuul's blog

Based on [Forever Jekyll - A simple, elegant & full featured Jekyll theme](https://forever-jekyll.github.io)

## Installation

See topics/development/tools/jekyll/_posts/2022-03-08-jekyll-basics.md

## Local testing

```
$ source ~/.bashrc
$ bundle install
$ bundle exec jekyll serve
```

Browse <http://127.0.0.1:4000/>

## Format converting

```
pandoc test2.odt -t markdown -o test2.md
```

## ToC

https://github.com/toshimaru/jekyll-toc

Gemfile: gem 'jekyll-toc'
_config.yml: 
plugins:
  - jekyll-toc

---
layout: post
title: "Welcome to Jekyll!"
toc: true
---

## Hierarchy

https://simpleit.rocks/ruby/jekyll/tutorials/how-to-create-breadcrumbs-with-hierarchical-categories-in-jekyll/

