---
layout: post
title: Jekyll static site generator Basics
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: https://jekyllrb.com/
* Version: 4.2.2
* File(s): --

# Installation

Debian/Ubuntu <https://jekyllrb.com/docs/installation/ubuntu/>:

Install Ruby and other prerequisites:

```sh
$ sudo apt-get install ruby-full build-essential zlib1g-dev
```

Avoid installing RubyGems packages (called gems) as the root user.
Instead, set up a gem installation directory for your user account.
The following commands will add environment variables to your `~/.bashrc` file to configure the gem installation path:

```sh
$ echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
$ echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
$ echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
$ source ~/.bashrc
```

Finally, install Jekyll and Bundler:

```sh
$ gem install jekyll bundler
```

# Usage

## Local testing

```
$ cd your_project_dir
$ source ~/.bashrc
$ bundle install
$ bundle exec jekyll serve
```

Open browser at <http://127.0.0.1:4000>