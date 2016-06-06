# AABB v AABB intersection

One to the most useful intersection tests is testing an AABB against another AABB. In fact, this is probably the intersection test you will use the most!

Surprisingly, this is also one of the simplest collision tests to write, it's a basic overlap test. Becuase the AABB has 3 axis, we just have to check each axis for an overlap. Consider the following image:

![TEST](aabb_moz_test.png)

It shows an overlap test on only the X axis. If either the X, Y or Z have any overlap we know we have an intersection. Representing the above test in code is as follows:

```cs
a.minX <= b.maxX && a.maxX >= b.minX
```

## The Algorithm

Actually implementing the above test is just a matter of extrapolating the single axis test onto multiple axis. Very similar to the Point in AABB test.

```cs
bool Intersect(AABB a, AABB b) {
  return (a.minX <= b.maxX && a.maxX >= b.minX) &&
         (a.minY <= b.maxY && a.maxY >= b.minY) &&
         (a.minZ <= b.maxZ && a.maxZ >= b.minZ);
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
code
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```