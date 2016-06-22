# Ray Against Plane

Testing a ray against a plane is surprisingly easy. Remember, any point on the ray can be represented by a time t, such that:

```
point(t) = plane.position + plane.normal * t
^ the (t) above means point at time t
```

Also, recall we can test if a point is on a plane if it fits this equation:

```
DOT(point, plane.normal) = plane.distance
```