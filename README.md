# gwlua

A middleware to write Game & Watch-like games in [Lua](http://www.lua.org/). Provides pictures, sprites, timers and sounds.

**gwlua** is a *middleware*, meaning it known nothing about how sound is actually played and sprites blitted onto the screen. You have to provide functions to do that according to the platform you're targetting.

For an working example of **gwlua**, see gw-libretro.

## Usage

Add gwlua.c and gwlua.h to your project. See gwlua.h for some documentation.

## Rationale

When I started porting [MADrigal](http://www.madrigaldesign.it/sim/)'s Game&Watch simulators to [libretro](http://www.libretro.com/), I had to make a decision on how to take advantage of the existing simulators' source code. The simulators are written in Delphi, so I could:

1. Try to compile the original source code with [Free Pascal](http://www.freepascal.org/) to some machine code (I was thinking about MIPS) and add a CPU emulator to the libretro core
2. Rewrite the original source code in another language

With 1, I could package the machine code along with the images and sounds and have a proper content, which is how the libretro core should work, but I'd have to bootstrap high level functions to deal i.e. with picture loading and blitting.

With 2, I could write everything in, say, C, and have an working game without too much hassle, but then a core would be specific to one G&W simulation only.

So I decided to go with an option that I believe has the best of the two: rewrite the Pascal code in Lua, and package the Lua source code along with assets into a libretro content. The libretro core would then be able to read the archive and run the Lua code, which in turn can easily call native functions to deal with IO etc.

## Why Lua

I have experience with both the language and on embedding it on C/C++ programs, and it's syntax is similar to that of Pascal. In fact, I wrote a translator that takes Pascal source code and generates Lua source code. The translator is far from perfect, but it does the hard work, leaving me to just tweak a few lines.
