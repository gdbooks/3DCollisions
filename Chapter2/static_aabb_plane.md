# AABB Plane intersection

To test if an AABB and plane intersect, we first have to project each vertex of the AABB onto the plane's normal. This leaves us with all the vertices of the AABB on a line.

We then check the vertex that is furthest from the plane. If the vertex diagonally opposite that one is on the other side of the plane, we have an intersection. 

This might be a bit hard to visualize, so i rendered it out step by step:



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