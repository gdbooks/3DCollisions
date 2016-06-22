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

That's it! We now have T! The above might be a bit confusing, let's take a look at it as just math. In the below formula __pos__ is the position of the ray, __dir__ is the normal of the ray. __norm__ is the normal of the plane and __dist__ is the distance of the plane. __t__ is of course t. t__dir__ means t * dir.

* First we have the formula for a plane
* $$unknownPoint \cdot norm = dist$$
*   
* Next, we substitute the point on ray formula for the unknown point
* $$(pos + t\mathbf d\mathbf i \mathbf r) \cdot norm = dist$$
*   
* Distribute the dot product
* $$pos \cdot norm + t\mathbf d\mathbf i\mathbf r \cdot norm = dist$$
*  
* Subtract pos dot norm from both sides
* $$t\mathbf d\mathbf i\mathbf r \cdot norm = dist - pos \cdot nrom$$
* 
* Divide both sides by dir dot norm
* $$t = \frac{dist - pos \cdot nrom}{dir \cdot norm}$$

A few things to keep in mind.

If the ray is parallel to the plane $$dir \cdot norm$$ will be 0 and there is no intersection.

An intersection is only valid if the ray is in front of the plane.That is, if the direction of the ray is opposite of the planes normal. This is true if $$dir \cdot norm < 0$$.

If the value of t is out of range (that is, if t is negative), then there is no intersection.