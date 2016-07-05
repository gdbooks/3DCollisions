# Plane Triangle

Plane triangle intersections are fairly simple. There are two use cases

1. Use the dot-product to determine whether the triangle lies fully on one side of the plane and does not intersect the plane at all.
2. If there is an intersection, use a line-plane-intersection-algorithm for the two edges hitting the plane (algorithm on the same page)

Thats it. It's a lot like AABB-V-Plane, where we just tested if every point of the AABB was on the same side of the plane.

## The Algorithm

```cs
code
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