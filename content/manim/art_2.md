---
title: "Refraction through a lens with Manim"
date: 2021-11-04
slug: "optics"
description: "Optics with Manim!"
keywords: []
draft: false
tags: [manim]
math: false
toc: false
---





Here’s an optics animation I made a while ago! 👁️👁️👓

<img src="/vid/EllipseExample.gif" style="zoom:200%;"/>



The code:

```python
from manim import *
import numpy as np

class EllipseExample(Scene):
    def construct(self):
        DashedLine.set_default(color=BLACK)
        self.camera.background_color = WHITE
        
        # lists which will hold the lines
        inc_list = []
        ref_list = []
		
        # the lens
        ellipse = Ellipse(
            width=2.0,
            height=6.5,
            stroke_color=BLACK,
            fill_color=[BLUE_A, BLUE_B],
            fill_opacity=1,
        ).shift(LEFT * 2)
        
        # used some math to calculate the focal lengh based on the size of the ellipse
        focal_length = 1 / (0.49 * (1 / ellipse.width - 1 / ellipse.height))
        end = ellipse.get_center() + RIGHT * focal_length
        

        # create a list of angles at which the dashed lines will enter the lens

        for angle in [x for x in np.arange(100 * DEGREES, 260 * DEGREES, 25 * DEGREES)]:
            # translate an angle along the ellipse into an actual point
            lens_point = ellipse.point_at_angle(angle)
            
            # create the incident ray.
            incident_ray = DashedLine(
                # start from the left side of the screen
                start=lens_point + config.left_side,
                end=lens_point,
                stroke_width=1,
            )
            
            distance = end - lens_point
            # used to make the lines appear really long
            last_value = lens_point + (distance * 2)
            
			
            refracted_ray = DashedLine(
                start=lens_point,
                end=last_value,
                stroke_width=1,
            )
            
            # add these lines to a list
            inc_list.append(incident_ray)
            ref_list.append(refracted_ray)

        self.add(ellipse)
        
        # animate them
        self.play(
            *[Create(inc, run_time=1.5, rate_func=linear) for inc in inc_list]
        )
        self.play(
            *[Create(ref, run_time=1.5, rate_func=linear) for ref in ref_list]
        )
        self.wait()
```

