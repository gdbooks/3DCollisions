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