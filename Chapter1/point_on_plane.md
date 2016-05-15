# Point on plane

Testing if a point is on a plane or not is fairly straight forward. In fact, we've done it multiple time. The key to this is the plane equasion!

```cs
// Plane representation
class Plane {
  float A, B, C; // Normal
  float D; // Dostance
}

// Plane equasion (XYZ is an unknown point)
A*X + B*Y + C*Z + D = 0

// Equasion can also be written as
Dot(UnknownPoint, Normal) == Distance
```

Given this information we can figure out if a point is on a plane or not.

We already know A, B, C & D. That is our plane. And the point X, Y, Z is provided because that's what we are testing. If we plug all these variables into the equasion:

```cs
A*X + B*Y + C*Z + D
```

The resulting number is what we care about. 

* If it is 0, the point is on the plane
* If it is positive, the point is in front of the plane
* If it is negative, the point is behind the plane

## On Your Own

Add the following functions to the ```Collisions``` class:

```cs
public static bool PointOnPlane(Point point, Plane plane);
// Should return a positive number, a negative number or 0
public static float PointPlaneEquasion(Point point, Plane plane);
```

And provide an implementation for them!

### Unit Test

