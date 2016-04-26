#Point in Sphere

Testing if a point is inside a sphere is one of the simplest 3D tests. Take a look at this image, and see if you can figure out the algorithm to perform the test.

#TODO

The actual algorithm is very simple. If the distance from the point to the center of the sphere is less than the radius of the sphere, the sphere contains the point!

You could achieve this by accessing the position of the sphere and point being tested as vectors. Subtract one vector from the other (order doesn't matter) and take the length of the resulting vector.

```cs

```