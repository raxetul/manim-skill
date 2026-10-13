---
name: manim
description: Create math, algorithm, and physics explainer animations with Manim Community Edition (ManimCE, the `manim` Python package). Use this skill when the user asks to "animate" an idea, mentions "manim", or wants an explainer video, a visual proof, an algorithm walk-through, a function plot, or a physics demo. The skill gives the install and render commands, a scene skeleton, the core Mobjects and animations, layout rules, updaters, camera control, a build workflow, and a table of common errors. It does not cover ManimGL (the 3Blue1Brown version).
license: MIT
---

# Manim Community Edition

Use this skill to write Python scenes that Manim renders to video or GIF.
All code uses `from manim import *`. The skill covers ManimCE only, not ManimGL.

## When to use

- The user wants a short animation of a math, algorithm, or physics idea.
- The user wants to see a proof, a plot, or a data structure change step by step.
- Do not use this skill for UI animation or for video editing.

## Install and render

Install Manim, ffmpeg, and a LaTeX distribution first. `MathTex` and `Tex` need LaTeX.

```bash
pip install manim
manim --version
```

Render a scene with the `manim` command. The file name comes first, then the scene name.

| Goal | Command |
|---|---|
| Low quality, open preview | `manim -pql scene.py MyScene` |
| Medium quality | `manim -pqm scene.py MyScene` |
| High quality (1080p) | `manim -qh scene.py MyScene` |
| GIF output | `manim -ql --format gif scene.py MyScene` |
| Last frame only (PNG) | `manim -ql -s scene.py MyScene` |
| All scenes in a file | `manim -ql -a scene.py` |

Manim writes the output to `media/videos/<file>/<quality>/MyScene.mp4`.
Images go to `media/images/`. Delete `media/` to clear the cache.

## Scene skeleton

Every animation is a class with a `construct` method.

```python
from manim import *


class Hello(Scene):
    def construct(self):
        title = Text("Hello, Manim")   # create a Mobject
        self.play(Write(title))        # animate it
        self.wait(1)                   # hold the last frame
        self.play(FadeOut(title))
```

- `self.add(m)` shows a Mobject at once, with no animation.
- `self.play(...)` runs one or more animations. Default `run_time` is 1 second.
- `self.wait(t)` pauses for `t` seconds.
- `self.remove(m)` removes a Mobject at once.

The frame is 14.2 units wide and 8 units high. The origin is the center.
Constants: `UP`, `DOWN`, `LEFT`, `RIGHT`, `ORIGIN`, `UL`, `UR`, `DL`, `DR`.

## Core Mobjects

| Kind | Examples |
|---|---|
| Text | `Text("plain")`, `MathTex(r"e^{i\pi}+1=0")`, `Tex(r"Some \textbf{text}")` |
| Shapes | `Circle()`, `Square()`, `Rectangle(width=3, height=1)`, `Triangle()`, `Dot()`, `Line(LEFT, RIGHT)`, `Arrow(LEFT, RIGHT)`, `Polygon(...)` |
| Groups | `VGroup(a, b, c)` |
| Axes | `Axes(...)`, `NumberPlane()`, `NumberLine()` |
| Data | `Table(...)`, `Matrix([[1, 2], [3, 4]])`, `BarChart(...)` |

Set style with arguments or with methods.

```python
box = Square(side_length=2, color=BLUE, fill_opacity=0.5)
box.set_stroke(WHITE, width=4)
label = MathTex(r"a^2 + b^2 = c^2").set_color(YELLOW).scale(0.8)
group = VGroup(box, label)   # move or animate both as one
```

Use `Text` for plain words. Use `MathTex` for formulas. `MathTex` content is already in math mode.

### Axes and graphs

```python
axes = Axes(
    x_range=[-3, 3, 1],
    y_range=[-1, 9, 1],
    x_length=6,
    y_length=4,
    axis_config={"include_numbers": True},
)
curve = axes.plot(lambda x: x**2, color=GREEN)
curve_label = axes.get_graph_label(curve, label="x^2")
dot = Dot(axes.c2p(2, 4), color=RED)   # c2p: coordinates to point
area = axes.get_area(curve, x_range=(0, 2), color=BLUE, opacity=0.4)
```

## Animations

| Animation | Use |
|---|---|
| `Create(m)` | Draw a shape stroke by stroke |
| `Write(m)` | Write text or a formula |
| `FadeIn(m)`, `FadeOut(m)` | Fade a Mobject in or out. Add `shift=UP` to slide it |
| `Transform(a, b)` | Change `a` into the shape of `b`. `a` stays on screen |
| `ReplacementTransform(a, b)` | Replace `a` with `b`. `b` stays on screen |
| `TransformMatchingTex(a, b)` | Morph one formula into another, part by part |
| `Indicate(m)`, `Circumscribe(m)` | Draw attention to `m` |
| `m.animate.<method>` | Animate any Mobject method |

The `.animate` syntax is the most useful tool. Chain the methods you want.

```python
sq = Square()
self.play(sq.animate.shift(RIGHT * 2).rotate(PI / 4).set_color(RED))
self.play(sq.animate.scale(0.5), run_time=2, rate_func=smooth)
```

`rate_func` sets the speed curve. Common values: `smooth` (default), `linear`,
`there_and_back`, `ease_in_out_sine`, `rush_into`, `rush_from`.

### Run animations together

```python
dots = VGroup(*[Dot() for _ in range(5)]).arrange(RIGHT, buff=0.5)

# Start each animation 0.2 of its length after the previous one
self.play(LaggedStart(*[FadeIn(d, shift=UP) for d in dots], lag_ratio=0.2))

# Run different animations at the same time
self.play(AnimationGroup(dots.animate.set_color(RED), Write(Text("Done").to_edge(UP))))
```

Pass several animations to one `self.play` call to run them together.

## Layout

| Method | Effect |
|---|---|
| `m.move_to(point)` | Put the center of `m` at `point` |
| `m.shift(2 * RIGHT + UP)` | Move `m` by a vector |
| `m.to_edge(UP, buff=0.5)` | Put `m` at a frame edge |
| `m.to_corner(UL)` | Put `m` in a corner |
| `a.next_to(b, DOWN, buff=0.3)` | Put `a` below `b` with a gap |
| `m.align_to(b, LEFT)` | Align one edge with `b` |
| `group.arrange(RIGHT, buff=0.4)` | Line up the children in a row |
| `group.arrange_in_grid(rows=2, cols=3)` | Place the children in a grid |
| `m.scale(1.5)` | Resize `m` |
| `m.get_center()`, `m.get_top()` | Read a position |

```python
title = Text("Sorting").to_edge(UP)
bars = VGroup(*[Rectangle(width=0.6, height=h) for h in (1, 3, 2, 4)])
bars.arrange(RIGHT, buff=0.2, aligned_edge=DOWN).next_to(title, DOWN, buff=1)
```

## Updaters and ValueTracker

A `ValueTracker` holds one number. An updater redraws a Mobject on each frame from that number.

```python
class Rotating(Scene):
    def construct(self):
        plane = NumberPlane()
        angle = ValueTracker(0)
        dot = Dot(color=YELLOW)
        # Place the dot on the unit circle at the tracked angle
        dot.add_updater(lambda m: m.move_to(
            plane.c2p(np.cos(angle.get_value()), np.sin(angle.get_value()))
        ))
        readout = always_redraw(lambda: DecimalNumber(angle.get_value())
                                .to_corner(UR))
        self.add(plane, dot, readout)
        self.play(angle.animate.set_value(2 * PI), run_time=4, rate_func=linear)
        dot.clear_updaters()
```

- `add_updater(fn)` calls `fn(mobject)` on each frame. Use `fn(mobject, dt)` for time steps.
- `always_redraw(fn)` rebuilds a whole Mobject on each frame.
- Call `clear_updaters()` when the motion ends.

## Camera

Use `MovingCameraScene` to zoom and pan in 2D.

```python
class Zoom(MovingCameraScene):
    def construct(self):
        square = Square()
        self.add(square)
        self.play(self.camera.frame.animate.scale(0.5).move_to(square.get_corner(UR)))
```

Use `ThreeDScene` for 3D. Set the view with `set_camera_orientation`.

```python
class Cube3D(ThreeDScene):
    def construct(self):
        self.set_camera_orientation(phi=70 * DEGREES, theta=-45 * DEGREES)
        self.add(ThreeDAxes())
        cube = Cube(side_length=2, fill_opacity=0.6)
        self.play(Create(cube))
        self.begin_ambient_camera_rotation(rate=0.3)
        self.wait(3)
        self.stop_ambient_camera_rotation()
```

Fix text to the screen in 3D with `self.add_fixed_in_frame_mobjects(label)`.

## Workflow

1. Plan the beats. Write one sentence for each idea the viewer must learn.
2. Build one scene (or one clear section) for each beat.
3. Keep each beat short: 5 to 20 seconds.
4. Render at low quality with `manim -pql`. Check layout, timing, and text size.
5. Fix errors and overlaps. Add `self.wait(1)` after each key step.
6. Render at high quality with `manim -qh` only when the low render is correct.
7. Join scenes with ffmpeg, or put the beats in one scene class.

Rules for clear animations:

- Show one new thing at a time.
- Keep the text short. Use at most 3 lines on screen.
- Use color to link ideas. Give each quantity the same color every time.
- Remove old Mobjects before the scene gets crowded.

## Common errors

| Symptom | Cause | Fix |
|---|---|---|
| `FileNotFoundError: latex` or `dvisvgm` | No LaTeX distribution | Install TeX Live, MiKTeX, or MacTeX. Restart the shell |
| `FileNotFoundError: ffmpeg` | ffmpeg is not on `PATH` | Install ffmpeg. Run `ffmpeg -version` |
| LaTeX error in `Tex` with `$...$` or `\frac` | `Tex` is text mode | Use `MathTex` for formulas, or wrap the math in `$...$` |
| `MathTex` fails on `\text{}` or a `%` sign | LaTeX special character | Escape it (`\%`) or put the words in `\text{...}` |
| Object does not move after `Transform` | `Transform(a, b)` keeps `a` on screen, not `b` | Use `ReplacementTransform`, or keep working with `a` |
| Mobject missing after `ReplacementTransform(a, b)` | `a` is gone. `b` is now on screen | Use `b` in the next animation |
| Object is invisible or cut off | It is outside the 14.2 x 8 frame | Use `scale`, `to_edge`, or `arrange`. Print `m.get_center()` |
| Same Mobject animated twice at once | Two animations act on one object | Use one `.animate` chain, or `AnimationGroup` |
| `.animate` does nothing | You called a method without `self.play` | Pass `m.animate.method()` to `self.play` |
| Text looks blurry | Low render quality | Render with `-qh` |
| Old output after a code change | Cached partial movie files | Add `--disable_caching` or delete `media/` |
