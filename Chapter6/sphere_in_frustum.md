# Sphere In Frustum

Doing the sphere in frustum is pretty easy. We already have a ```HalfSpaceTest``` function in our collision class. We want to check the distance of the sphere's space to every plane of the frustum. If the distance between the sphere and any frustum plane is less than the negative radius of the sphere (That is to say, its further behind the plane than the radius of the sphere), no collision has happened

## The Algorithm

```cs
for each plane of the frustum
  space = HalfSpaceTest(sphere.center, plane);
  // If the distance is < radius, the sphere is outside
  if (space < -radius)
    return false; // We are outside!
return true; // We're in
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool Intersects(Plane[] frustum, Sphere sphere) {
    // TODO: Implement this
}

public static bool Intersects(Sphere sphere, Plane[] frustum) {
    return Intersects(frustum, sphere);
}
```

And provide an implementation for it!

### Unit Test

Add the following unit test to the end of the ```Initialize``` function of ```CameraSample``` scene

```cs
if (!Collisions.Intersects(new Sphere(), camera.Frustum)) {
    System.Console.WriteLine("Error with sphere in frustum! 0");
}
if (Collisions.Intersects(new Sphere(-500, -500, -500, 1f), camera.Frustum)) {
    System.Console.WriteLine("Error with sphere in frustum! 1");
}
if (!Collisions.Intersects(new Sphere(2, 30, 4, 4f), camera.Frustum)) {
    System.Console.WriteLine("Error with sphere in frustum! 2");
}
```