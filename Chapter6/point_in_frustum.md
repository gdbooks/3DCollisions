# Point In Frustum

Perhaps the easyest frustum test to perform is the Point In Frustum test. Actually, we've [implemented this functionality](https://gdbooks.gitbooks.io/legacyopengl/content/Chapter8/frustum.html) before. The only difference is, now we will formally include it in our collision library.

## The Algorithm

The algorithm here is simple, it's essentially just a half-space test. A half space test is very similar to the ```DistanceFromPlane``` function we wrote, except instead of adding plane.Distance at the end, we subtract it. Once you can do a half-space test, doing the actual intersection is easy, loop trough all of the frustum planes, and do a half space test between the point and each plane. If any of the half space tests are less than 0, return false immediateley. IF they are all 0 or greater you can return true by default.

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static float HalfSpaceTest(Point point, Plane plane) {
    // TODO: Implement this!
}
    
public static bool Intersects(Plane[] frustum, Point point) {
    // TODO: Implement this!
}

public static bool Intersects(Point point, Plane[] frustum) {
    return Intersects(frustum, point);
}
```

And provide an implementation for it!

### Unit Test

Add the following unit test at the end of the ```Initialize``` function in ```CameraSample```.

```cs
if (!Collisions.Intersects(new Point(), camera.Frustum)) {
    System.Console.WriteLine("Error with point in frustum!");
}
if (Collisions.Intersects(new Point(-500,-500,-500), camera.Frustum)) {
    System.Console.WriteLine("Error with point in frustum!");
}
if (!Collisions.Intersects(new Point(2, 30, 4), camera.Frustum)) {
    System.Console.WriteLine("Error with point in frustum!");
}
```