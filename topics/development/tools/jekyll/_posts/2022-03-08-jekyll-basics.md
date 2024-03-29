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

## Editing Posts

### Link to another internal post

Use full path with date being converted to directories:

```sh
[conversion tools](/topics/linux/2011/09/30/linux-file-conversion)
```

### Link to heading inside same page

Activate Kramdown auto ids (see <https://stackoverflow.com/a/48539931/643690>):

File `_config.yml`:

```
kramdown:
    auto_ids: true
```

With this enabled each heading gets an id ref based on the heading text. For example

```
### My Funky Heading
```

will become

```
<h3 id="my-funky-heading">My Funky Heading</h3>
```

You can link to this from within the same document by doing something like this:

```
The funky text is [described below](#my-funky-heading)
```

You can assign an explicit id if you prefer:

```
### My Funky Heading
{: #funky }
```

and link to it

```
The funky text is [described below](#funky)
```