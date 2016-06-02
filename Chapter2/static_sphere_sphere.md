# Sphere Sphere Intersection

The overlap test between two spheres is very simple. The distance between the sphere centers is computed and compared against the sum of the sphere radii. To
avoid an often expensive square root operation, the squared distances are compared.
The test looks like this: