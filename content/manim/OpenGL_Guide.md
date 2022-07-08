---
title: "Manim OpenGL Renderer Usage Guide - November 2021"
date: 2021-10-29
slug: "opengl_guide"
description: "How to use the Manim OpenGL Renderer."
keywords: ["manim"]
draft: false
tags: ["manim"]
math: false
toc: false
---



# OpenGL Guide - Updated November 2021

## Why use the OpenGL renderer?

- ***Significant*** speed improvement for rendering scenes.
  - Shifted from rendering with `Cairo` to shaders with OpenGL
  - Shaders are GPU-accelerated whereas Cairo runs purely on the CPU
- Live interactive window which allows for:
  - Manoeuvring with the camera
  - Manipulating the scene after rendering via `self.interactive_embed()`
  - Greatly improved debugging and usability
  - Also can (sort of) do presentations, not properly implemented yet.
- GUI which lets you interact with the scene / switch to other scenes with ease.
- OpenGL is the future of Manim.

> **Important**: the window only lasts for the duration of the animation, and will close immediately afterwards without `self.interactive_embed()`. Therefore, a scene without animations (just `self.add()`) will instantly close.

## Main Flags

- `--renderer=opengl`
  -  The principal flag to use the opengl renderer. 
  - `-p`**must** be passed to activate the interactive window
- `--write_to_movie` produces an mp4 file.
  - Moving the camera via the interactive window **will** impact the mp4 produced.
- `--use_projection_stroke_shaders` + `--use_projection_fill_shaders`
  - Flags to use the rewritten 3b1b shaders by EulerTour [#1716](https://github.com/ManimCommunity/manim/pull/1716). 
  - Make `VMobjects` compatible with projection matrices.
  - Shader code is much easier to understand than 3b1b’s shaders.
  - Alternative to Grant’s shaders, which have been shown to have mysterious bugs and interactions
    -  Obscure glitching / artefacting [#1762](https://github.com/ManimCommunity/manim/issues/1762)
    - Stroke shaders not able to be rendered by dedicated Intel GPU’s
  - Currently are not anti-aliased, so mobjects may appear pixelated if examined closely
  - **Unknowns**: performance difference.
- `--window_position`
  - Accepts  a direction (LEFT, RIGHT, UR, UL)  and positions the interactive window accordingly
  - Also accepts tuple coordinates (400,400) to position the top-left corner of the window 
- `--window_size`
  - The window automatically adjusts to take up a quarter of your monitor, but can also be manually adjusted.



## `self.interactive_embed()`

- Incredibly useful 
- Adding a line: `self.interactive_embed()` within your scene allows you to interact with the scene directly via an `IPython` shell

```python
class OpenGLShow(Scene):
    def construct(self):
        circ = Circle()
        square = Square()
        self.add(circ, square)
        self.interactive_embed()
```

- Can interact with the scene and its mobjects: (`self.<method>` is NOT needed and will NOT work )
  - `add(Circle()) `: adds a circle to the scene
  - `remove(circ) ` will remove the original circle from the scene
  - `square.get_center()` will return the center of the original square
  - `play(Write(Axes())` will animate `Axes`being added to a scene
  - `play(square.animate.scale(5))` will animate the scaling of the square.



## “Will my code work with OpenGL?” 

- **Answer**: most likely.
- OpenGL `Mobjects` and regular `Mobjects` have a very different backend and are **NOT** compatible with each other.
- **Our solution**: compatibility via metaclasses:
  - Throughout the codebase, you might notice something like: `

```python
class VGroup(VMobject, metaclass=ConvertToOpenGL)
```

- This tells Manim to use the different base Mobjects depending on the renderer used.
  - `VMobject` --> `OpenGLVMobject`
  - `Mobject` --> `OpenGLVMobject`
  - Surface --> `OpenGLSurface`
- ***IMPORTANT***: calling `VMobject`/`Mobject` /`Surface` directly will ***BREAK*** `OpenGL` scenes, use `OpenGLMobject` /`OpenGLVMobject`/`OpenGLSurface` instead.

### Currently known code that is not yet duo-compatible:

- `ImageMobject`, in progress at [#1837](https://github.com/ManimCommunity/manim/pull/1837)
- There may be other code that is not compatible, if you find some, please let us know!

### Troubleshooting

> Nothing is rendering!

- There are some known issues depending on the type of hardware on your machine. This might be a case of strokes not being able to be rendered using the shaders Grant has written. We aren’t completely sure why this happens; however, the projection shaders should help resolve the issue. 

> My scenes aren’t running any faster

- Unfortunately, there are some performance issues that particularly affect the OpenGL renderer. This means that you might notice that OpenGL code runs even slower than Cairo. We’re working on solving this issue, but when we eventually transition to the projection shaders, most of these performance issues should be resolved as we can more efficiently render scenes.

  

## GUI + Flags

- Manim supports a GUI interface, currently not very easy to use, but it is possible, (see `example_scenes/opengl.py/GuiTest`)
- DearPyGUI must be explicitly installed first (`pip install dearpygui`) then can be enabled via  `--enable_gui`
- Allows for:
  - Live scene switching
  - Rerunning a scene after rendering is complete
  - Manipulating properties of mobjects in the scene (see `GuiTest2`)
    - Currently *very* tedious to do.
- `--gui_location` to control position of the GUI window



#### Recap:

`pip install dearpygui`

`--enable_gui` to activate the GUI

**Image** :

<img src="/img/opengl_showcase.png" alt="opengl_showcase" style="zoom:200%;" />
