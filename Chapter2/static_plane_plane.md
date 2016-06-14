# Plane Plane intersection

We've already done some work with plane intersection, when we did the intersection for 3 planes onto a point. Luckly the intersection of 2 planes is a lot simpler.

The only time two planes don't intersect is if they are parallel. If the planes are not parallel they will intersect in a line (of infinate length)

![int](intersection_of_planes.png)

The line of intersection is fairly useless in game development, so we will not be covering it here.

## The Algorithm

The key piece of information above is that two planes never intersect if they are parallel. Only if they are parallel. Knowing this case, the problem becomes, how can we tell if two planes are parallel?

Dot Product to the rescue! We have the normal of each plane! If the normals point in the same direction, we have parallel planes!

```cs
bool PlanesIntersect(Plane p1, Plane p2) {
    // Compute direction of intersection line
    Vector d = Cross(p1.n, p2.n);
    
    // If the length(Squared) of d is zero, the planes are 
    // parallel (and separated) or coincident, 
    // so they’re not considered intersecting
    return (Dot(d, d) > EPSILON);
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
code
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/StaticIntersections.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```