---
layout: post
title: Linux File Conversion
author: Ralf Eichinger
toc: true
---

# Installation

## ImageMagick

```sh
$ sudo apt install imagemagick
```

## Djvu

```sh
$ sudo apt install djvulibre-bin
```

# PDF to JPG/PNG

Use `convert` from ImageMagick:

```sh
$ convert Das_Weltall.pdf Das_Weltall.jpg
```

or

```sh
$ convert Das_Weltall.pdf Das_Weltall.png
```

# JPG to PDF

```sh
$ for i in *.jpg; do convert $i $i.pdf; done
```

# DJVU to TIFF

When using an indirect djvu file, use the name of the index file:

```sh
$ ddjvu -format=tiff Weltall\ u.\ Menschheit\ 1.djvu Weltall_u._Menschheit_1.tif
```

# TIFF to JPG

TIFF containing only one picture

```sh
$ for i in *.tif; do convert "$i" "$i.jpg"; done
```

TIFF containing multiple pictures

```sh
$ convert -separate example.tif example.jpg
```
