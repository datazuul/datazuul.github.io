---
layout: post
title: FLTK-Tutorial
author: Ralf Eichinger
toc: true
---

Einführung in die Programmierung mit FLTK (Fast Light Toolkit).
Homepage: <https://www.fltk.org/>

# Ein Fenster

Quellcode (fenster.cpp):

```c
#include <FL/Fl.H>
#include <FL/Fl_Window.H>

int main(int argc, char **argv)
{
  Fl_Window mainWin(300, 150, "Fenster");
  mainWin.end();
  mainWin.show(argc, argv);
  return Fl::run();
}
```

Kompilieren:

```sh
$ gcc `fltk-config --cxxflags --ldflags` -o fenster fenster.cpp
```

Ausführen:

```sh
$ ./fenster
```

Screenshot:

<figure>
<img src="{{site.baseurl}}/assets/topics/development/fltk/fenster.jpg" alt="Ein FLTK-Fenster" />
</figure>

# Hello World

Quellcode (hello.cpp):

```c
// include required header files
#include <FL/Fl.H>
#include <FL/Fl_Window.H>
#include <FL/Fl_Box.H>

int main(int argc, char **argv) {
  // create window with width=300, height=180 (in pixel)
  Fl_Window *window = new Fl_Window(300,180);

  // create a box with the "Hello, World!" string in it
  // Fl_Widget(x, y, width, height, label)
  // position of box: x=20, y=20 in window (from top left corner)
  // size of box: width=260, height=100
  // label: Hello, World! 
  Fl_Box *box = new Fl_Box(20,40,260,100,"Hello, World!");

  // set type of box
  // default: FL_NO_BOX
  box->box(FL_UP_BOX);

  // set height of label/font
  box->labelsize(36);

  // set style of label/font
  box->labelfont(FL_BOLD+FL_ITALIC);
  box->labeltype(FL_SHADOW_LABEL);

  // finally show window
  window->end();
  window->show(argc, argv);

  // enter FLTK event loop
  return Fl::run();
}
```

Kompilieren:

```sh
$ gcc `fltk-config --cxxflags --ldflags` -o hello hello.cpp
```

Ausführen:

```sh
$ ./hello
```

Screenshot:

<figure>
<img src="{{site.baseurl}}/assets/topics/development/fltk/hello_world.jpg" alt="Hello World" />
</figure>