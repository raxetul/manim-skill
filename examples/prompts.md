# Sample prompts

Paste a prompt into your agent. Each prompt tells the manim skill what to build.

## 1. Pythagorean proof

```text
Use the manim skill. Animate a visual proof of the Pythagorean theorem with four
copies of a right triangle inside a square. Show a^2 + b^2 = c^2 at the end.
```

Expect one scene, `PythagoreanProof`, about 30 seconds. Render with `manim -pql proof.py PythagoreanProof`.

## 2. Binary search walk-through

```text
Use the manim skill. Show binary search on the sorted list [1, 3, 5, 7, 9, 11, 13].
Find 11. Mark low, mid, and high. Grey out the half that the search drops.
```

Expect one scene, `BinarySearch`, about 25 seconds. Render with `manim -pql search.py BinarySearch`.

## 3. Sine as a rotating point

```text
Use the manim skill. Draw a unit circle next to a sine graph. A point moves around
the circle. Its height draws the sine curve in real time.
```

Expect one scene, `SineFromCircle`, built with a `ValueTracker` and updaters, about 20 seconds.
Render with `manim -pql sine.py SineFromCircle`.

## 4. Matrix as a transform of the plane

```text
Use the manim skill. Show the matrix [[2, 1], [0, 1]] as a transform of the plane.
Draw the basis vectors i and j, and show where they land.
```

Expect one scene, `MatrixTransform`, using `NumberPlane` and `ApplyMatrix`, about 15 seconds.
Render with `manim -pql matrix.py MatrixTransform`.

## 5. Bubble sort

```text
Use the manim skill. Animate bubble sort on the list [5, 2, 4, 1, 3] with bars.
Highlight each pair that the algorithm compares. Swap the bars when needed.
```

Expect one scene, `BubbleSort`, about 40 seconds. Render a GIF with `manim -ql --format gif sort.py BubbleSort`.

## 6. Gradient descent on a curve

```text
Use the manim skill. Plot f(x) = x^2 - 2x + 3. Put a dot at x = 3 and move it
downhill in 6 steps. Show the tangent line at each step.
```

Expect one scene, `GradientDescent`, about 25 seconds. Render with `manim -pql descent.py GradientDescent`.

## 7. Compound interest growth

```text
Use the manim skill. Compare linear growth and exponential growth on one set of axes.
Label both curves. Add a line that shows where the exponential curve passes the linear one.
```

Expect one scene, `GrowthCompare`, about 20 seconds. Render with `manim -qh growth.py GrowthCompare`.

## 8. Projectile motion

```text
Use the manim skill. Show a ball in a parabolic arc. Draw the velocity vector at
three points. Split each vector into horizontal and vertical parts.
```

Expect one scene, `ProjectileMotion`, about 25 seconds. Render with `manim -pql projectile.py ProjectileMotion`.

## Check a prompt result

Parse the generated file before you render it.

```python
import ast
import sys

ast.parse(open(sys.argv[1]).read())
print("syntax ok")
```
