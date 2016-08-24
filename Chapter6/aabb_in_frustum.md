# AABB In Frustum

Testing if an AABB is within a frustum is a bit of a brute force method. We first have to get every vertex of the AABB, this part is simple enough. We alrady did this when we [split the BVH tree](https://gdbooks.gitbooks.io/3dcollisions/content/Chapter4/bvh_split.html). We just need the 8 points.

Once we have the 8 points it's a nested loop. 

For every plane of the frustum, we check all 8 corners of the AABB. If there is any plane where all 8 corners of the AABB are outside of the plane (Have a negative half-space test), then the AABB and frustum do not intersect!

By default, if everything passes we assume an intersection to be true

## The Algorithm

```cs
foreach Plane plane in frustum {
    int inCount = 8;
    foreach Point corner in corners{
        if HalfSpaceTest(corner, plane) < 0.0f {
            inCount -= 1;
        }
    }
    if inCount <= 0
        return false;
}
return true; // by default
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool Intersects(Plane[] frustum, AABB aabb) {
    // TODO
}

public static bool Intersects(AABB aabb, Plane[] frustum) {
    return Intersects(frustum, aabb);
}
```

And provide an implementation for it!

### Unit Test

Add the following unit test to the bottom of the ```Initialize``` function in ```CameraSample```

```cs
if (!Collisions.Intersects(new AABB(), camera.Frustum)) {
    System.Console.WriteLine("Error with aabb in frustum! 0");
}
if (Collisions.Intersects(new AABB(new Point(-650, -650, -650), new Point(-600, -600, -600)), camera.Frustum)) {
    System.Console.WriteLine("Error with aabb in frustum! 1");
}
if (!Collisions.Intersects(new AABB(new Point(2, 30, 4), new Point(25, 35, 45)), camera.Frustum)) {
    System.Console.WriteLine("Error with aabb in frustum! 2");
}
```