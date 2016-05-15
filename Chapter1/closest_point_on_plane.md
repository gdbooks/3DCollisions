# Closest Point on Plane

Sometimes knowing if a point is on a plane or not isn't enough. Imagine your character walking in a room. You want the feet of your character to be on the floor. This is where knowing the closest point on plane helps. You can get your characters position, and the closest point on the ground plane is where you need to place him.

Technically, the we find the closest point to a plane using an orthographic projection, but there is an easyer way to understand this.

![P1](cp_plane1.gif)

Here we have a plane with a normal N. We want to find the closest point on the plane to P0, on the image that would be Px. Take note, the point Px is some distance away from P0, but that distance is in the direciton of the normal!

The closest point on a plane will always be in the direction of the plane normal from the test point. All we have to figure out is __how far is the test point from the plane__? In the above picture, that distance is __d__.

### The algorithm

Thanks to the above image, we can deduct that given a plane, and a point, the closest point on the plane from that point will be the point minus the distance of the point from the plane in the direction of the planes normal.

The question now becomes, how do we find the distance between a point and a plane? The last section _Point on Plane_ mentioned this as the formula:

```cs
Dot(SomePoint, Normal) == Distance
```

This becomes pretty easy to implement in a function like so:

```cs
// THIS BLOCK IS JUST SAMPLE CODE, DON'T COPY IT!
Point ClosestPointOnPlane(Plane plane, Point point) {
    // This works assuming plane.Normal is normalized, which it should be
    float distance = DOT(plane.Normal, point) - plane.Distance;
    // If the plane normal wasn't normalized, we'd need this:
    // distance = distance / DOT(plane.Normal, plane.Normal);
    
    return point - distance * plane.Normal;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static Point ClosestPoint(Plane plane, Point point);
```

And provide an implementation for it!

### Unit Test

#TODO: Download

The following code is visual only, if you make any mistakes no error is printed!

![UNIT](aabb_closest_point_unit.png)

```cs
TODO
```