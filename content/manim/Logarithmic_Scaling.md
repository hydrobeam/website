---
title: "Implementing Logarithmic Scaling in Manim"
date: 2021-10-06
slug: "log"
description: "Some thoughts I had while working on this feature."
keywords: ["manim"]
draft: false
tags: ["manim"]
math: false
toc: false
---

 

To help breathe some life into this website, I decided it might be nice to discuss my experience with implementing  [this PR](https://github.com/ManimCommunity/manim/pull/1812) which introduces log scaling for `Axes` in Manim. 

Here’s the result:

<img src="/img/image-20211006222818131.png" alt="image-20211006222818131" style="zoom:200%;" />

```python
class LSEE(Scene):
    def construct(self):
        self.camera.background_color = WHITE
        ax = Axes(
            x_range=[0.0001, 10.0001, 1],  # min must be > 0, log is undefined at 0.
            y_range=[-2, 4, 1],
            tips=False,
            axis_config={"include_numbers": True, "exclude_origin_tick": False},
            y_axis_config={"scaling": LogBase(custom_labels=True)},
        )

        graph = ax.get_graph(
            lambda x: x ** 2,
            use_smoothing=False,
            x_range=(ax.x_range[0], ax.x_range[1], 0.001),
        )
        self.add(
            Tex(
                "Plot of $x$ on a logarithmic curve", color=BLACK, font_size=24
            ).to_edge(UP)
        )
        self.add(ax, graph)
```

and its code.



## Challenge

This was really the first time I had a lot of difficulty implementing something in code. Although, I wasn’t “inventing” anything new (log curves have been around at least since the 90’s), there were several concepts I was forced to consider that I wasn’t really accustomed to. The first was the actual *implementation*.



### Implementation

The most obvious part of a coding problem: *“how the hell do I even get this to work?!”*. Well, given that most of my code experience up to this point had been relatively simple (or have had the challenge localized into a very small file), I simply jumped right in and hoped my brain would carry me to the finish line.



It didn’t.



Why not? Well it’s kind of challenging on the surface, isn’t it? How was I going to transform code that was exclusively built for linear plots to accommodate log plots? I vaguely recalled hearing something about linear interpolation, so I figured there must be some equivalent *logarithmic interpolation* which would cast away all my worries. A brief search later, I found this link: https://www.cmu.edu/biolphys/deserno/pdf/log_interpol.pdf. In there, you can find a lovely little formula which seems like it’d solve all your problems. Unfortunately, on top of me (probably) not understanding when to apply the formula, actually trying to stitch out the linear interpolation code with this snippet causes everything to crash and burn. I don’t even recall the errors at this point, but I distinctly recall the misery they caused.



With that out of the way, I realized this wasn’t going to be as simple as changing a couple lines of code and tossing in a parameter: there was going to have to be some major reworking. Eventually, I realized that what I really wanted was still linear interpolation, but I just wanted the numbers to be different. I introduced a separate “holder” class which would scale the numbers that passed into the interpolating function so that they follow a logarithmic pattern. I affectionately called this class `LogBase`.



> `LogBase` inherits from `_ScaleBase` which acts like a template which tells a dev (or user) how to implement similar `XBase` for different types of curves.



#### Composition over Inheritance 👀

The `LogBase` class followed a system of composition over inheritance. I read about it in a blog about it and it seemed like all the rage. It seemed like I had a good shot at implementing a composition-esque approach so I did, and it turned out pretty well.  Side note: I am **very** against the notion of “always composition over inheritance”. It just doesn’t make sense!!! Something just *have* to inherit from each other, a car is simply a Vehicle, a Square is simply a `Mobject`. Maybe  I have a gross misunderstanding of when to apply composition, but I watched a talk claiming that you can always implement composition over inheritance, and `attrs` didn’t even support inheritance that well up until some random update which let you configure a custom `__post_init__`, and *even then*, they don’t even let you change the order of the MRO?? What if I need to start from the top to bottom, like it literally works normally when you inherit from classes? Ok enough ranting, back to the point.

  



🚧Under-Construction🚧

