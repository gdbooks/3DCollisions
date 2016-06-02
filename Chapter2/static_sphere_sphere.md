# Sphere Sphere Intersection

The overlap test between two spheres is very simple. The distance between the sphere centers is computed and compared against the sum of the sphere radii. To avoid an often expensive square root operation, the squared distances are compared.

The test looks like this:

![INT](NhwT6.png)

The blue line is the distance between the center of the spheres. In the first example this line is greater than the sum radii of the spheres, they do not intersect. The second example, it's equal, they intersect. The third and fourth examples see the distance be less than the sum of the radii as well, they also intersect.

The above example fails to show the radii of the spheres, in the below image, the yellow line does that.

![NIT](sphtosph.png)

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

The constructor of this code will spit out errors if they are present. A ray is rendered, along with a random sampling of points. Any point that falls on the ray is rendered in red, points not on the ray are rendered in blue.

![UNIT](point_on_ray_sample_01.PNG)

```cs