# TOPIC

Desctiption with images

## The Algorithm

The code to test intersecting spheres is super straight forward.

```cs
bool TestSphereSphere(Sphere a, Sphere b) {
    // Calculate squared distance between centers
    Vector d = a.position - b.position;
    float squaredDistance = Dot(d, d); // Same as d.LengthSquared()
    
    // Spheres intersect if squared distance is less than squared sum of radii
    float radiusSum = a.radius + b.radius;
    float squaredRadii = radiusSum * radiusSum;
    
    return squaredDistance <= squaredRadii;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool Intersects(Sphere s1, Sphere s2)
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```