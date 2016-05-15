# Point on plane

Testing if a point is on a plane or not is fairly straight forward. In fact, we've done it multiple time. The key to this is the plane equasion!

```cs
// Plane representation
class Plane {
  float A, B, C; // Normal
  float D; // Dostance
}

// Plane equasion (XYZ is a point on the plane)
A*X + B*Y + C*Z + D = 0

// This means that the distance (D) of the point from 
// the plane can be represented as:
Dot(UnknownPoint, Normal) == Distance
```

Given this information we can figure out if a point is on a plane or not. Let's rearrange the distance equasion, so the distance is on the left side and the right side is 0:

```cs
Dot(UnknownPoint, Normal) - Distance == 0
```

If a point satisfies the above equasion it is on the plane!

We already know A, B, C & D. That is our plane. And the point X, Y, Z is provided because that's what we are testing. If we plug all these variables into the equasion:

```cs
// NOTE the -D, this is NOT the plane equasion
// It is the modified distance equasion!
A*X + B*Y + C*Z - D // DOT(ABD, XYZ) - D
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

# TODO: DOWNLOAD

The unit test renders a large plane and colors the dots around it. Any point on the plane is green, points behind the plane are red, points in front of it are blue. The constructor will spit out errors if either of the above equasions is bad.

![UNIT](aabb_closest_point_unit.png)

```cs

```