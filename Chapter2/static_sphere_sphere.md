# Sphere Sphere Intersection

The overlap test between two spheres is very simple. The distance between the sphere centers is computed and compared against the sum of the sphere radii. To avoid an often expensive square root operation, the squared distances are compared.

The test looks like this:

![INT](NhwT6.png)

The blue line is the distance between the center of the spheres. In the first example this line is greater than the sum radii of the spheres, they do not intersect. The second example, it's equal, they intersect. The third and fourth examples see the distance be less than the sum of the radii as well, they also intersect.

The above example fails to show the radii of the spheres, in the below image, the yellow line does that.

![NIT](sphtosph.png)

## The Algorithm


```cs
bool TestSphereSphere(Sphere a, Sphere b) {
    // Calculate squared distance between centers
    Vector d = a.c - b.c;
    float squaredDistance = Dot(d, d);
    
    // Spheres intersect if squared distance is less than squared sum of radii
    float radiusSum = a.r + b.r;
    float squaredRadii = radiusSum * radiusSum;
    
    return squaredDistance <= squaredRadii;
}
```