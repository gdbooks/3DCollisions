# AABB Plane intersection

To test if an AABB and plane intersect, we first have to project each vertex of the AABB onto the plane's normal. This leaves us with all the vertices of the AABB on a line.

We then check the vertex that is furthest from the plane. If the vertex diagonally opposite that one is on the other side of the plane, we have an intersection. 

This might be a bit hard to visualize, so i rendered it out step by step:



## The Algorithm

This test is equivalent to finding an AABB vertex most distant along the plane
normal and making sure that vertex and the vertex diagonally opposite lie on opposite
sides of the plane.

```cs
// Test if AABB b intersects plane p
int TestAABBPlane(AABB b, Plane p) {
    // Convert AABB to center-extents representation
    Point c = (b.max + b.min) * 0.5f; // Compute AABB center
    Point e = b.max - c; // Compute positive extents
    
    // Compute the projection interval radius of b onto L(t) = b.c + t * p.n
    float r = e[0]*Abs(p.n[0]) + e[1]*Abs(p.n[1]) + e[2]*Abs(p.n[2]);
    
    // Compute distance of box center from plane
    float s = Dot(p.n, c) - p.d;
    
    // Intersection occurs when distance s falls within [-r,+r] interval
    return Abs(s) <= r;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
// TODO: Provide implementation for this
public static bool Intersects(AABB aabb, Plane plane) 

// Conveniance function
public static bool Intersects(Plane plane, AABB aabb) {
    return Intersects(aabb, plane);
}

```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```