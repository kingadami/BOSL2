# Basic Shapes Tutorial

## Enhanced Primitives
There are 5 built-in primitive shapes that OpenSCAD provides.  The BOSL2
library extends these shapes so that they support more features, and more
ways to simply reorient them.

### Enhanced 2D Square
You can still use `square()` in the familiar ways that OpenSCAD provides:
```openscad-example
    square(100, center=false);
```

```openscad-example
    square(100, center=true);
```

```openscad-example
    square([40,50], center=true);
```

BOSL2's enhanced `square()` also allows you to round the corners:
```openscad-example
    square([40,50], center=true, rounding=10);
```

It also supports chamfers:
```openscad-example
    square([40,50], center=true, chamfer=10);
```

It even allows you to specify *which* corners get rounded or chamferred:
```openscad-example
    square([40,50], center=true, rounding=[0,5,10,15]);
```
```openscad-example
    square([40,50], center=true, chamfer=[0,5,10,15]);
```

It will even let you mix rounding and chamferring:
```openscad-example
    square([40,50], center=true, rounding=[5,0,10,0], chamfer=[0,5,0,15]);
```

### Anchors and Spin

The `anchor=` argument is an alternative to `center=`, which allows more
alignment options.  It takes a vector as a value, pointing roughly towards
the side or corner you want to align to the origin.  For example, to align
the center of the back edge to the origin, set the anchor to `[0,1]`:
```openscad-example
    square([40,50], anchor=[0,1]);
```

To align the front right corner to the origin:
```openscad-example
    square([40,50], anchor=[1,-1]);
```

To center:
```openscad-example
    square([40,50], anchor=[0,0]);
```

To make it clearer when giving vectors, there are several standard vector constants defined:
- `LEFT` is a vector towards X-.  (`[-1,0,0]`)
- `RIGHT` is a vector towards X+.  (`[1,0,0]`)
- `FRONT`, `FORWARD` or `FWD` is a vector towards Y-.  (`[0,-1,0]`)
- `BACK` is a vector towards Y+.  (`[0,1,0]`)
- `BOTTOM`, `BOT`, `BTM`  or `DOWN` is a vector towards Z-.  (`[0,0,-1]`)  Not used with 2D anchors.
- `TOP`, or `UP` is a vector towards Z+.  (`[0,0,1]`)  Not used with 2D anchors.
- `CENTER` or `CTR` is a centered vector. (`[0,0,0]`)

Note that even though these are 3D vectors, you can use most of them, (except `UP`/`DOWN`, of course) for anchors in 2D shapes:
```openscad-example
    square([40,50], anchor=BACK);
```

```openscad-example
    square([40,50], anchor=CENTER);
```

You can add them together to point to corners:
```openscad-example
    square([40,50], anchor=FRONT+RIGHT);
```

Finally, the `spin` argument can rotate the shape by a given number of degrees clockwise:
```openscad-example
    square([40,50], anchor=CENTER, spin=30);
```

Anchoring or centering is performed before the spin:
```openscad-example
    square([40,50], anchor=BACK, spin=30);
```

### Enhanced 2D Circle
The enhanced `circle()` primitive can be used like the OpenSCAD built-in:
```openscad-example
    circle(r=50);
```
```openscad-example
    circle(d=100);
```
```openscad-example
    circle(d=100, $fn=8);
```

Since a circle in OpenSCAD can only be approximated by a regular polygon with
a number of straight sides, this can lead to size and shape inaccuracies.  To
counter this, the `realign` and `circum` arguments are also provided.

The `realign` argument, if set `true`, rotates the circle by half the angle between sides:
```openscad-example
    circle(d=100, $fn=8, realign=true);
```

The `circum` argument, if true, makes the polygon describing the circle circumscribe the ideal circle instead of inscribing it.

Inscribing the ideal circle:
```openscad-example
    difference() {
        circle(d=100, $fn=360);
        circle(d=100, $fn=6);
    }
```

Circumscribing the ideal circle:
```openscad-example
    difference() {
        circle(d=100, $fn=6, circum=true);
        circle(d=100, $fn=360);
    }
```

You can also use anchor and spin on enhanced `circle()`:
```openscad-example
    circle(r=50, anchor=BACK);
```

```openscad-example
    circle(r=50, anchor=FRONT+RIGHT);
```

Using spin on a circle may not make initial sense, until you remember that anchoring is performed before spin:
```openscad-example
    circle(r=50, anchor=FRONT, spin=30);
```

### Echanced 3D Cube
You can use enhanced `cube()` like the normal OpenSCAD built-in:
```openscad-example
    cube(100);
```
```openscad-example
    cube(100, center=true);
```
```openscad-example
    cube([50,40,20], center=true);
```

You can use `anchor` similarly to `square()`, except you can anchor vertically
too, in 3D, allowing anchoring to faces, edges, and corners:
```openscad-example
    cube([50,40,20], anchor=BOTTOM);
```
```openscad-example
    cube([50,40,20], anchor=TOP+BACK);
```
```openscad-example
    cube([50,40,20], anchor=TOP+FRONT+LEFT);
```

You can use `spin` as well, to rotate around the Z axis:
```openscad-example
    cube([50,40,20], anchor=FRONT, spin=30);
```

3D objects also gain the ability to use an extra trick with `spin`;
if you pass a list of `[X,Y,Z]` rotation angles to `spin`, it will
rotate by the three given axis angles, similar to using `rotate()`:
```openscad-example
    cube([50,40,20], anchor=FRONT, spin=[15,0,30]);
```

3D objects also can be given an `orient` argument that is given as a vector,
pointing towards where the top of the shape should be rotated towards.
```openscad-example
    cube([50,40,20], orient=UP+BACK+RIGHT);
```

If you use `anchor`, `spin`, and `orient` together, the anchor is performed
first, then the spin, then the orient:
```openscad-example
    cube([50,40,20], anchor=FRONT, spin=45, orient=UP+FWD+RIGHT);
```

### Echanced 3D Cylinder
You can use the enhanced `cylinder()` as normal for OpenSCAD:
```openscad-example
    cylinder(r=50,h=50);
```
```openscad-example
    cylinder(r=50,h=50,center=true);
```
```openscad-example
    cylinder(d=100,h=50,center=true);
```
```openscad-example
    cylinder(d1=100,d2=80,h=50,center=true);
```

