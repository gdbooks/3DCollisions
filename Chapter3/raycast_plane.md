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

A ray and a plane collide if a point exists that satisfies both of these equations! Let's start out with the formula for point on plane:

```
DOT(point, plane.normal) = plane.distance
```

In this formula the point is unknown. But we know that the point must also be on the ray at some time t. So, let's substitute the unknown point for the point on ray formula.

```
DOT(ray.position + ray.normal * t, plane.normal) = plane.distance
```

At this point, the only unknown in the above equation is t. Let's start re-arranging the equation so t is on the left side by its-self. We can distribute the dot product operation:

```
DOT(ray.position, plane.normal) + DOT(ray.normal * t, plane.normal) = plane.distance
```

Now, we can subtract the first dot product from each side.

```
DOT(ray.normal * t, plane.normal) = plane.distance - DOT(ray.position, plane.normal);
```

Finally, we can divide each side by ray.normal DOT plane.normal.

```
t = (plane.distance - DOT(ray.position, plane.normal) / DOT(ray.normal, plane.normal)
```