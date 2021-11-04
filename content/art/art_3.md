---
title: "Perlin Noise Vs Random Distrubition - Visualization"
date: 2021-11-04
slug: "perlin"
description: "An animation to highlight the difference between pseudo-random and rando."
keywords: []
draft: false
tags: [manim]
math: false
toc: false
---

Here’s an animation I made when I was learning about Perlin Noise! I used the [`perlin-noise`](https://pypi.org/project/perlin-noise/)/`random` libraries to generate the data.



<img src="/vid/PerlinNoiseVsRandom.gif" style="zoom:200%;"/>



The code:



```python
from perlin_noise.perlin_noise import PerlinNoise
import numpy as np
from manim import *
import random


class PerlinNoiseVsRandom(Scene):
    def construct(self):
        # make manim light mode
        DecimalNumber.set_default(color=BLACK)
        MathTex.set_default(color=BLACK)
        NumberLine.set_default(color=BLACK)
        self.camera.background_color = WHITE        

        # generate the axes
        ax = Axes(
            x_range=[0, 1, 0.1],
            y_range=[-0.5, 0.5, 0.1],
            x_length=9,
            y_length=6.5,
            axis_config={"font_size": 36},
            y_axis_config={"numbers_with_elongated_ticks": [-0.5, 0.5]},
            x_axis_config={"numbers_with_elongated_ticks": [1]},
            tips=False,
        ).add_coordinates([0, 1], [-0.5, 0.5])
        
        labels = ax.get_axis_labels()
        self.add(labels)
        self.add(ax)

        perlin_noise = Tex("Perlin Noise").to_edge(UR).shift(UP * 0.3)
        random_sequence = Tex("Random Distribution").to_edge(UR).shift(UP * 0.3)

        noise_array = []
        rand_array = []
        
        # initialize the PerlinNoise
        noise = PerlinNoise(octaves=4)
        x = range(0, 101)
        length = len(x)
        
        # put the data into arrays to put into the VMobjects
        for i in x:
            # setup
            x_value = i / (length)
            y_value_perlin = noise(x_value)
            y_value_random = random.uniform(ax.y_range[0], ax.y_range[1])
            
            # job: [1, 2] --> point on the axes
            position_rand = ax.c2p(x_value, y_value_random)
            position_perl = ax.c2p(x_value, y_value_perlin)
            noise_array.append(position_perl)
            rand_array.append(position_rand)
		
        # the perlin noise / random curves
        poly_perlin = VMobject(stroke_width=2, color=BLUE_C).set_points_as_corners(
            noise_array
        )
        poly_rand = VMobject(stroke_width=2, color=RED).set_points_as_corners(
            rand_array
        )

        self.wait(0.5)

        self.play(Write(perlin_noise))
        self.play(Write(poly_perlin), run_time=3.5)

        self.wait(0.25)

        self.play(Unwrite(perlin_noise), Unwrite(poly_perlin))

        self.wait(0.25)

        self.play(Write(random_sequence))
        self.play(Write(poly_rand), run_time=3.25)

        self.play(FadeOut(Group(*self.mobjects)), run_time=0.75)

        self.wait(0.25)

```





