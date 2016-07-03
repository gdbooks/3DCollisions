# Sphere triangle test

Testing for the intersection of a sphere and a trianlge is amazingly simple. Get the closest point on the triangle to the center of the sphere. Check the distance (squared) between the center of the sphere and this point. If the distance is less than the sphere's radius (squared), then there is a collision.

## The Algorithm

This one is so simple, i'm not going to be providing sample code for it. 

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
// TODO: Implement this function
public static bool Intersects(Triangle triangle, Sphere sphere)

// This is just a conveniance function
public static bool Intersects(Sphere sphere, Triangle triangle) {
    return Intersects(triangle, sphere);
}
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/3DModels.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```