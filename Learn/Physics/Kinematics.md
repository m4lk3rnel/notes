  $x_0$  - initial position
  $x$ - final position
  $v$ - final velocity
  $v_0$ - initial velocity
  $a$ - constant acceleration
  $t$ - time
- **core formula**: $x=x_0​+v_0​t+\frac12​at^2$

- Example problem: **A rock is dropped from the top of a 50-meter tall cliff.**  How long does it take to reach the ground, and what is its speed when it hits?

- $x_0=50m$ (starting height)
- $x=0m$ (ground level)
- $v_0=0*\frac ms$ (dropped, not thrown)
- $a=-9.8 * \frac m{s^2}$ (downward acceleration, earth's gravitational acceleration)
- $t=?,v=?$

$x=x_0​+v_0​t+\frac12​at^2$
$0=50+0*t+\frac 12(-9.8)t^2$
$0=50-4.9t^2$
$4.9t^2=50$q
$t^2=\frac {50}{4.9}\approx 10.2$
$t\approx \sqrt {10.2} \approx {3.2}seconds$

---
### Vectors

- a **vector** has a **magnitude** (the size of the vector) and a **direction**. 
- you can find the magnitude (the length, size of the vector) using the Pythagorean theorem: $a^2 + b^2 = c^2$ 
	or the **distance formula**: $d=\sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$

- you can calculate the direction of the vector using: $\theta=tan^{-1}(\frac yx)$ (gives the correct angle only in **quadrants I and IV**), or `atan2(y,x)` which gives the correct direction in all quadrants.

-  Great question! The **components of a vector** are the individual values that represent how far the vector extends in each direction (axis).

- you can find the components if you know the **magnitude** and the **direction**:
	- $x = |\vec{v}| * cos(\theta)$
	- $y = |\vec {v}| * sin(\theta)$