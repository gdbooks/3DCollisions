# Sphere Plane Intersection

This is a pretty simple intersection logic, like with the Sphere-AABB intersection, we've already written the basic checks to support it. To see if a sphere and plane intersect:

1. Find the closest point on the plane to the sphere
2. Make sure the distance of that point is <= than the sphere radius

That's it. Of course you might want to check length squared against radius squared to save on some performance.

## The Algorithm

This is a super simple algorithm (about 3 lines), try implementing it without a code guide.

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
// TODO: Provide implementation
public static bool Intersects(Sphere sphere, Plane plane) 

// Conveniance function
public static bool Intersects(Plane plane, Sphere sphere) {
    return Intersects(sphere, plane);
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