# Sphere In Frustum

Doing the sphere in frustum is pretty easy. We already have a ```DistanceFromPlane``` function in our collision class. We want to check the distance of the sphere to every plane of the frustum. If the distance between the sphere and any frustum plane is less than the negative radius of the sphere (That is to say, its further behind the plane than the radius of the sphere), no collision has happened

## The Algorithm

```cs
for each plane of the frustum
  distance = DistanceFromPlane(sphere.center, plane);
  // If the distance is < radius, the sphere is outside
  if (distance < -radius)
    return false; // We are outside!
return true; // We're in
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